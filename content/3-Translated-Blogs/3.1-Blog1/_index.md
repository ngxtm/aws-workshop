---
title : "Blog 1"
date :  2025-09-10 
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

## Build multi-agent site reliability engineering assistants with Amazon Bedrock AgentCore

*by Amit Arora and Dheeraj Oruganty | on 26 SEP 2025 | in [Amazon Bedrock](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/), [Amazon Bedrock Agents](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/amazon-bedrock-agents/), [Amazon Bedrock Knowledge Bases](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/amazon-bedrock/amazon-bedrock-knowledge-bases/), [Amazon Machine Learning](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/amazon-machine-learning/), [Artificial Intelligence](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/), [Generative AI](https://aws.amazon.com/blogs/machine-learning/category/artificial-intelligence/generative-ai/), [Technical How-to](https://aws.amazon.com/blogs/machine-learning/category/post-types/technical-how-to/)*

Site reliability engineers (SREs) face an increasingly complex challenge in modern distributed systems. During production incidents, they must rapidly correlate data from multiple sources—logs, metrics, Kubernetes events, and operational runbooks—to identify root causes and implement solutions. Traditional monitoring tools provide raw data but lack the intelligence to synthesize information across these diverse systems, often leaving SREs to manually piece together the story behind system failures.

With a generative AI solution, SREs can ask their infrastructure questions in natural language. For example, they can ask "Why are the payment-service pods crash looping?" or "What's causing the API latency spike?" and receive comprehensive, actionable insights that combine infrastructure status, log analysis, performance metrics, and step-by-step remediation procedures. This capability transforms incident response from a manual, time-intensive process into a time-efficient, collaborative investigation.

In this post, we demonstrate how to build a multi-agent SRE assistant using [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/), [LangGraph](https://langchain-ai.github.io/langgraph/), and the [Model Context Protocol (MCP)](https://modelcontextprotocol.io/). This system deploys specialized AI agents that collaborate to provide the deep, contextual intelligence that modern SRE teams need for effective incident response and infrastructure management. We walk you through the complete implementation, from setting up the demo environment to deploying on Amazon Bedrock AgentCore Runtime for production use.

## Solution overview

This solution uses a comprehensive multi-agent architecture that addresses the challenges of modern SRE operations through intelligent automation. The solution consists of four specialized AI agents working together under a supervisor agent to provide comprehensive infrastructure analysis and incident response assistance.

The examples in this post use synthetically generated data from our demo environment. The backend servers simulate realistic Kubernetes clusters, application logs, performance metrics, and operational runbooks. In production deployments, these stub servers would be replaced with connections to your actual infrastructure systems, monitoring services, and documentation repositories.

The architecture demonstrates several key capabilities:

- **Natural language infrastructure queries** – You can ask complex questions about your infrastructure in plain English and receive detailed analysis combining data from multiple sources
- **Multi-agent collaboration** – Specialized agents for Kubernetes, logs, metrics, and operational procedures work together to provide comprehensive insights
- **Real-time data synthesis** – Agents access live infrastructure data through standardized APIs and present correlated findings
- **Automated runbook execution** – Agents retrieve and display step-by-step operational procedures for common incident scenarios
- **Source attribution** – Every finding includes explicit source attribution for verification and audit purposes

The following diagram illustrates the solution architecture.

![AWS AgentCore architecture showing SRE support agent workflow with API monitoring and authentication components](/images/translated-blogs/blog1/pic1.png)

The architecture demonstrates how the SRE support agent integrates seamlessly with Amazon Bedrock AgentCore components:

- **Customer interface** – Receives alerts about degraded API response times and returns comprehensive agent responses
- **Amazon Bedrock AgentCore Runtime** – Manages the execution environment for the multi-agent SRE solution
- **SRE support agent** – Multi-agent collaboration system that processes incidents and orchestrates responses
- **Amazon Bedrock AgentCore Gateway** – Routes requests to specialized tools through OpenAPI interfaces:
  - Kubernetes API for getting cluster events
  - Logs API for analyzing log patterns
  - Metrics API for analyzing performance trends
  - Runbooks API for searching operational procedures
- **Amazon Bedrock AgentCore Memory** – Stores and retrieves session context and previous interactions for continuity
- **Amazon Bedrock AgentCore Identity** – Handles authentication for tool access using [Amazon Cognito](https://aws.amazon.com/cognito) integration
- **Amazon Bedrock AgentCore Observability** – Collects and visualizes agent traces for monitoring and debugging
- **Amazon Bedrock LLMs** – Powers the agent intelligence through Anthropic's Claude large language models (LLMs)

The multi-agent solution uses a supervisor-agent pattern where a central orchestrator coordinates five specialized agents:

- **Supervisor agent** – Analyzes incoming queries and creates investigation plans, routing work to appropriate specialists and aggregating results into comprehensive reports
- **Kubernetes infrastructure agent** – Handles container orchestration and cluster operations, investigating pod failures, deployment issues, resource constraints, and cluster events
- **Application logs agent** – Processes log data to find relevant information, identifies patterns and anomalies, and correlates events across multiple services
- **Performance metrics agent** – Monitors system metrics and identifies performance issues, providing real-time analysis and historical trending
- **Operational runbooks agent** – Provides access to documented procedures, troubleshooting guides, and escalation procedures based on the current situation

## Using Amazon Bedrock AgentCore primitives

The solution showcases the power of Amazon Bedrock AgentCore by using multiple core primitives. The solution supports two providers for Anthropic's LLMs. Amazon Bedrock supports Anthropic's Claude 3.7 Sonnet for AWS integrated deployments, and Anthropic API supports Anthropic's Claude 4 Sonnet for direct API access.

The Amazon Bedrock AgentCore Gateway component converts the SRE agent's backend APIs (Kubernetes, application logs, performance metrics, and operational runbooks) into Model Context Protocol (MCP) tools. This enables agents built with an open-source framework supporting MCP (such as LangGraph in this post) to seamlessly access infrastructure APIs.

Security for the entire solution is provided by Amazon Bedrock AgentCore Identity. It supports ingress authentication for secure access control for agents connecting to the gateway, and egress authentication to manage authentication with backend servers, providing secure API access without hardcoding credentials.

The serverless execution environment for deploying the SRE agent in production is provided by Amazon Bedrock AgentCore Runtime. It automatically scales from zero to handle concurrent incident investigations while maintaining complete session isolation. Amazon Bedrock AgentCore Runtime supports both OAuth and [AWS Identity and Access Management](https://aws.amazon.com/iam) (IAM) for agent authentication. Applications that invoke agents must have appropriate IAM permissions and trust policies. For more information, see [Identity and access management for Amazon Bedrock AgentCore.](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/security-iam.html)

Amazon Bedrock AgentCore Memory transforms the SRE agent from a stateless system into an intelligent learning assistant that personalizes investigations based on user preferences and historical context. The memory component provides three distinct strategies:

- **User preferences strategy** (`/sre/users/{user_id}/preferences`) – Stores individual user preferences for investigation style, communication channels, escalation procedures, and report formatting. For example, Alice (a technical SRE) receives detailed systematic analysis with troubleshooting steps, whereas Carol (an executive) receives business-focused summaries with impact analysis.
- **Infrastructure knowledge strategy** (`/sre/infrastructure/{user_id}/{session_id}`) – Accumulates domain expertise across investigations, enabling agents to learn from past discoveries. When the Kubernetes agent identifies a memory leak pattern, this knowledge becomes available for future investigations, enabling faster root cause identification.
- **Investigation memory strategy** (`/sre/investigations/{user_id}/{session_id}`) – Maintains historical context of past incidents and their resolutions. This enables the solution to suggest proven remediation approaches and avoid anti-patterns that previously failed.

The memory component demonstrates its value through personalized investigations. When both Alice and Carol investigate "API response times have degraded 3x in the last hour," they receive identical technical findings but completely different presentations.

Alice receives a technical analysis:

```python
memory_client.retrieve_user_preferences(user_id="Alice")
# Returns: {"investigation_style": "detailed_systematic_analysis", "reports": "technical_exposition_with_troubleshooting_steps"}
```

Carol receives an executive summary:

```python
memory_client.retrieve_user_preferences(user_id="Carol") 
# Returns: {"investigation_style": "business_impact_focused","reports": "executive_summary_without_technical_details"}
```

## Adding observability to the SRE agent

Adding observability to an SRE agent deployed on Amazon Bedrock AgentCore Runtime is straightforward using the Amazon Bedrock AgentCore Observability primitive. This enables comprehensive monitoring through [Amazon CloudWatch](https://aws.amazon.com/cloudwatch) with metrics, traces, and logs. Setting up observability requires three steps:

1. Add the OpenTelemetry packages to your `pyproject.toml`:

```toml
dependencies = [
    # ... other dependencies ...
    "opentelemetry-instrumentation-langchain",
    "aws-opentelemetry-distro~=0.10.1",
]
```

2. [Configure observability for your agents](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/observability-configure.html) to enable metrics in CloudWatch.
3. Start your container using the `opentelemetry-instrument` utility to automatically instrument your application.

The following command is added to the Dockerfile for the SRE agent:

```dockerfile
# Run application with OpenTelemetry instrumentation 
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

As shown in the following screenshot, with observability enabled, you gain visibility into the following:

- **LLM invocation metrics** – Token usage, latency, and model performance across agents
- **Tool execution traces** – Duration and success rates for each MCP tool call
- **Memory operations** – Retrieval patterns and storage efficiency
- **End-to-end request tracing** – Complete request flow from user query to final response

![AWS CloudWatch observability dashboard for SRE agent showing session metrics, trace counts, and FM token usage trends](/images/translated-blogs/blog1/gif1.gif)

The observability primitive automatically captures these metrics without additional code changes, providing production-grade monitoring capabilities out of the box.

## Development to production flow

The SRE agent follows a four-step structured deployment process from local development to production, with detailed procedures documented in [Development to Production Flow](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#development-to-production-deployment-flow) in the accompanying GitHub repo:

![The four-step structured deployment process](/images/translated-blogs/blog1/pic2.png)

The deployment process maintains consistency across environments: the core agent code (`sre_agent/`) remains unchanged, and the `deployment/` folder contains deployment-specific utilities. The same agent works locally and in production through environment configuration, with Amazon Bedrock AgentCore Gateway providing MCP tools access across different stages of development and deployment.

## Implementation walkthrough

In the following section, we focus on how Amazon Bedrock AgentCore Gateway, Memory, and Runtime work together to build this multi-agent collaboration solution and deploy it end-to-end with MCP support and persistent intelligence.

We start by setting up the repository and establishing the local runtime environment with API keys, LLM providers, and demo infrastructure. We then bring core AgentCore components online by creating the gateway for standardized API access, configuring authentication, and establishing tool connectivity. We add intelligence through AgentCore Memory, creating strategies for user preferences and investigation history while loading personas for personalized incident response. Finally, we configure individual agents with specialized tools, integrate memory capabilities, orchestrate collaborative workflows, and deploy to AgentCore Runtime with full observability.

Detailed instructions for each step are provided in the repository:

- **[Use Case Setup Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#use-case-setup)** – Backend deployment and development setup
- **[Deployment Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent/docs/deployment-guide.md)** – Production containerization and Amazon Bedrock AgentCore Runtime deployment

### Prerequisites

You can find the port forwarding requirements and other setup instructions in the README file's [Prerequisites](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent#prerequisites) section.

## Convert APIs to MCP tools with Amazon Bedrock AgentCore Gateway

Amazon Bedrock AgentCore Gateway demonstrates the power of protocol standardization by converting existing backend APIs into MCP tools that agent frameworks can consume. This transformation happens seamlessly, requiring only OpenAPI specifications.

### Upload OpenAPI specifications

The gateway process begins by uploading your existing API specifications to [Amazon Simple Storage Service](http://aws.amazon.com/s3) (Amazon S3). The [create_gateway.sh](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/create_gateway.sh) script automatically handles uploading the four API specifications (Kubernetes, Logs, Metrics, and Runbooks) to your configured S3 bucket with proper metadata and content types. These specifications will be used to create API endpoint targets in the gateway.

### Create an identity provider and gateway

Authentication is handled seamlessly through Amazon Bedrock AgentCore Identity. The [main.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/main.py) script creates both the credential provider and gateway:

```python
# Create AgentCore Gateway with JWT authorization
def create_gateway(
    client: Any,
    gateway_name: str,
    role_arn: str,
    discovery_url: str,
    allowed_clients: list = None,
    description: str = "AgentCore Gateway created via SDK",
    search_type: str = "SEMANTIC",
    protocol_version: str = "2025-03-26",
) -> Dict[str, Any]:
    
    # Build auth config for Cognito
    auth_config = {"customJWTAuthorizer": {"discoveryUrl": discovery_url}}
    if allowed_clients:
        auth_config["customJWTAuthorizer"]["allowedClients"] = allowed_clients
    
    protocol_configuration = {
        "mcp": {"searchType": search_type, "supportedVersions": [protocol_version]}
    }

    response = client.create_gateway(
        name=gateway_name,
        roleArn=role_arn,
        protocolType="MCP",
        authorizerType="CUSTOM_JWT",
        authorizerConfiguration=auth_config,
        protocolConfiguration=protocol_configuration,
        description=description,
        exceptionLevel='DEBUG'
    )
    return response
```

### Deploy API endpoint targets with credential providers

Each API becomes an MCP target through the gateway. The solution automatically handles credential management:

```python
def create_api_endpoint_target(
    client: Any,
    gateway_id: str,
    s3_uri: str,
    provider_arn: str,
    target_name_prefix: str = "open",
    description: str = "API Endpoint Target for OpenAPI schema",
) -> Dict[str, Any]:
    
    api_target_config = {"mcp": {"openApiSchema": {"s3": {"uri": s3_uri}}}}

    # API key credential provider configuration
    credential_config = {
        "credentialProviderType": "API_KEY",
        "credentialProvider": {
            "apiKeyCredentialProvider": {
                "providerArn": provider_arn,
                "credentialLocation": "HEADER",
                "credentialParameterName": "X-API-KEY",
            }
        },
    }
    
    response = client.create_gateway_target(
        gatewayIdentifier=gateway_id,
        name=target_name_prefix,
        description=description,
        targetConfiguration=api_target_config,
        credentialProviderConfigurations=[credential_config],
    )
    return response
```

### Validate MCP tools are ready for agent framework

Post-deployment, Amazon Bedrock AgentCore Gateway provides a standardized `/mcp` endpoint secured with JWT tokens. Testing the deployment with [mcp_cmds.sh](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/gateway/mcp_cmds.sh) reveals the power of this transformation:

```
Tool summary:
================
Total tools found: 21

Tool names:
• x_amz_bedrock_agentcore_search
• k8s-api___get_cluster_events
• k8s-api___get_deployment_status
• k8s-api___get_node_status
• k8s-api___get_pod_status
• k8s-api___get_resource_usage
• logs-api___analyze_log_patterns
• logs-api___count_log_events
• logs-api___get_error_logs
• logs-api___get_recent_logs
• logs-api___search_logs
• metrics-api___analyze_trends
• metrics-api___get_availability_metrics
• metrics-api___get_error_rates
• metrics-api___get_performance_metrics
• metrics-api___get_resource_metrics
• runbooks-api___get_common_resolutions
• runbooks-api___get_escalation_procedures
• runbooks-api___get_incident_playbook
• runbooks-api___get_troubleshooting_guide
• runbooks-api___search_runbooks
```

## Universal agent framework compatibility

This MCP-standardized gateway can now be configured as a Streamable-HTTP server for MCP clients, including AWS Strands, Amazon's agent development framework, LangGraph, the framework used in our SRE agent implementation, and CrewAI, a multi-agent collaboration framework.

The advantage of this approach is that existing APIs require no modification—only OpenAPI specifications. Amazon Bedrock AgentCore Gateway handles the following:

- **Protocol translation** – Between REST APIs to MCP
- **Authentication** – JWT token validation and credential injection
- **Security** – TLS termination and access control
- **Standardization** – Consistent tool naming and parameter handling

This means you can take existing infrastructure APIs (Kubernetes, monitoring, logging, documentation) and instantly make them available to AI agent frameworks that support MCP—through a single, secure, standardized interface.

## Implement persistent intelligence with Amazon Bedrock AgentCore Memory

Whereas Amazon Bedrock AgentCore Gateway provides seamless API access, Amazon Bedrock AgentCore Memory transforms the SRE agent from a stateless system into an intelligent, learning assistant. The memory implementation demonstrates how a few lines of code can enable sophisticated personalization and cross-session knowledge retention.

### Initialize memory strategies

The SRE agent memory component is built on Amazon Bedrock AgentCore Memory's event-based model with automatic namespace routing. During initialization, the solution creates three memory strategies with specific namespace patterns:

```python
from sre_agent.memory.client import SREMemoryClient
from sre_agent.memory.strategies import create_memory_strategies

# Initialize memory client
memory_client = SREMemoryClient(
    memory_name="sre_agent_memory",
    region="us-east-1"
)

# Create three specialized memory strategies
strategies = create_memory_strategies()
for strategy in strategies:
    memory_client.create_strategy(strategy)
```

The three strategies each serve distinct purposes:

- **User preferences** (`/sre/users/{user_id}/preferences`) – Individual investigation styles and communication preferences
- **Infrastructure Knowledge**: `/sre/infrastructure/{user_id}/{session_id}` – Domain expertise accumulated across investigations
- **Investigation Summaries**: `/sre/investigations/{user_id}/{session_id}` – Historical incident patterns and resolutions

### Load user personas and preferences

The solution comes preconfigured with user personas that demonstrate personalized investigations. The [manage_memories.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/scripts/manage_memories.py) script loads these personas:

```python
# Load Alice - Technical SRE Engineer
alice_preferences = {
    "investigation_style": "detailed_systematic_analysis",
    "communication": ["#alice-alerts", "#sre-team"],
    "escalation": {"contact": "alice.manager@company.com", "threshold": "15min"},
    "reports": "technical_exposition_with_troubleshooting_steps",
    "timezone": "UTC"
}

# Load Carol - Executive/Director
carol_preferences = {
    "investigation_style": "business_impact_focused",
    "communication": ["#carol-executive", "#strategic-alerts"],
    "escalation": {"contact": "carol.director@company.com", "threshold": "5min"},
    "reports": "executive_summary_without_technical_details",
    "timezone": "EST"
}

# Store preferences using memory client
memory_client.store_user_preference("Alice", alice_preferences)
memory_client.store_user_preference("Carol", carol_preferences)
```

### Automatic namespace routing in action

The power of Amazon Bedrock AgentCore Memory lies in its automatic namespace routing. When the SRE agent creates events, it only needs to provide the `actor_id`—Amazon Bedrock AgentCore Memory automatically determines which namespaces the event belongs to:

```python
# During investigation, the supervisor agent stores context
memory_client.create_event(
    memory_id="sre_agent_memory-abc123",
    actor_id="Alice",  # AgentCore Memory routes this automatically
    session_id="investigation_2025_01_15",
    messages=[("investigation_started", "USER")]
)

# Memory system automatically:
# 1. Checks strategy namespaces
# 2. Matches actor_id "Alice" to /sre/users/Alice/preferences
# 3. Stores event in User Preferences Strategy
# 4. Makes event available for future retrievals
```

### Validate the personalized investigation experience

The memory component's impact becomes clear when both Alice and Carol investigate the same issue. Using identical technical findings, the solution produces completely different presentations of the same underlying content.

Alice's technical report contains detailed systematic analysis for technical teams:

```
Technical Investigation Summary

Root Cause: Payment processor memory leak causing OOM kills

Analysis:
- Pod restart frequency increased 300% at 14:23 UTC
- Memory utilization peaked at 8.2GB (80% of container limit)
- JVM garbage collection latency spiked to 2.3s

Next Step:
1. Implement heap dump analysis (`kubectl exec payment-pod -- jmap`)
2. Review recent code deployments for memory management changes
3. Consider increasing memory limits and implementing graceful shutdown
```

Carol's executive summary contains business impact focused for executive stakeholders:

```
Business Impact Assessment
Status: CRITICAL - Customer payment processing degraded
Impact: 23% transaction failure rate, $47K revenue at risk
Timeline: Issue detected 14:23 UTC, resolution ETA 45 minutes
Business Actions: 
- Customer communication initiated via status page
- Finance team alerted for revenue impact tracking
- Escalating to VP Engineering if not resolved by 15:15 UTC
```

The memory component enables this personalization while continuously learning from each investigation, building organizational knowledge that improves incident response over time.

## Deploy to production with Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore makes it straightforward to deploy existing agents to production. The process involves three key steps: containerizing your agent, deploying to Amazon Bedrock AgentCore Runtime, and invoking the deployed agent.

### Containerize your agent

Amazon Bedrock AgentCore Runtime requires ARM64 containers. The following code shows the complete [Dockerfile](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/Dockerfile):

```dockerfile
# Use uv's ARM64 Python base image
FROM --platform=linux/arm64 ghcr.io/astral-sh/uv:python3.12-bookworm-slim

WORKDIR /app

# Copy uv files
COPY pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync --frozen --no-dev

# Copy SRE agent module
COPY sre_agent/ ./sre_agent/

# Set environment variables
# Note: Set DEBUG=true to enable debug logging and traces
ENV PYTHONPATH="/app" \
    PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

# Expose port
EXPOSE 8080

# Run application with OpenTelemetry instrumentation
CMD ["uv", "run", "opentelemetry-instrument", "uvicorn", "sre_agent.agent_runtime:app", "--host", "0.0.0.0", "--port", "8080"]
```

Existing agents just need a FastAPI wrapper (`agent_runtime:app`) to become compatible with Amazon Bedrock AgentCore, and we add `opentelemetry-instrument` to enable observability through Amazon Bedrock AgentCore.

### Deploy to Amazon Bedrock AgentCore Runtime

Deploying to Amazon Bedrock AgentCore Runtime is straightforward with the [deploy_agent_runtime.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/deployment/deploy_agent_runtime.py) script:

```python
import boto3

# Create AgentCore client
client = boto3.client('bedrock-agentcore', region_name=region)

# Environment variables for your agent
env_vars = {
    'GATEWAY_ACCESS_TOKEN': gateway_access_token,
    'LLM_PROVIDER': llm_provider,
    'ANTHROPIC_API_KEY': anthropic_api_key  # if using Anthropic
}

# Deploy container to AgentCore Runtime
response = client.create_agent_runtime(
    agentRuntimeName=runtime_name,
    agentRuntimeArtifact={
        'containerConfiguration': {
            'containerUri': container_uri  # Your ECR container URI
        }
    },
    networkConfiguration={"networkMode": "PUBLIC"},
    roleArn=role_arn,
    environmentVariables=env_vars
)

print(f"Agent Runtime ARN: {response['agentRuntimeArn']}")
```

Amazon Bedrock AgentCore handles the infrastructure, scaling, and session management automatically.

### Invoke your deployed agent

Calling your deployed agent is just as simple with [invoke_agent_runtime.py](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/deployment/invoke_agent_runtime.py):

```python
# Prepare your query with user_id and session_id for memory personalization
payload = json.dumps({
    "input": {
        "prompt": "API response times have degraded 3x in the last hour",
        "user_id": "Alice",  # User for personalized investigation
        "session_id": "investigation-20250127-123456"  # Session for context
    }
})

# Invoke the deployed agent
response = agent_core_client.invoke_agent_runtime(
    agentRuntimeArn=runtime_arn,
    runtimeSessionId=session_id,
    payload=payload,
    qualifier="DEFAULT"
)

# Get the response
response_data = json.loads(response['response'].read())
print(response_data)  # Full response includes output with agent's investigation
```

### Key benefits of Amazon Bedrock AgentCore Runtime

Amazon Bedrock AgentCore Runtime offers the following key benefits:

- **Zero infrastructure management** – No servers, load balancers, or scaling to configure
- **Built-in session isolation** – Each conversation is completely isolated
- **AWS IAM integration** – Secure access control without custom authentication
- **Automatic scaling** – Scales from zero to thousands of concurrent sessions

The complete deployment process, including building containers and handling AWS permissions, is documented in the [Deployment Guide](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/docs/deployment-guide.md).

## Real-world use cases

Let's explore how the SRE agent handles common incident response scenarios with a real investigation.

When facing a production issue, you can query the system in natural language. The solution uses Amazon Bedrock AgentCore Memory to personalize the investigation based on your role and preferences:

```bash
export USER_ID=Alice
sre-agent --prompt "API response times have degraded 3x in the last hour"
```

The supervisor retrieves Alice's preferences from memory (detailed systematic analysis style) and creates an investigation plan tailored to her role as a Technical SRE:

```
Investigation Plan
1. Use metrics_agent to analyze API performance metrics including response times, error rates, and resource utilization to identify the extent and pattern of slowdown
2. Use logs_agent to examine application logs for errors related to slow API responses, focusing on database connection issues and memory errors
3. Use kubernetes_agent to check pod status and resource constraints, especially for web-service and database pods, looking for CrashLoopBackOff states and missing ConfigMaps
Complexity: Simple
Auto-execute: Yes
Agents involved: Metrics Agent, Logs Agent, Kubernetes Agent
```

The agents investigate sequentially according to the plan, each contributing their specialized analysis. The solution then aggregates these findings into a comprehensive executive summary:

```
Executive Summary

Key Insights
- Root Cause: Database service failure due to missing ConfigMap 'database-config' in production namespace, causing database pod to crash repeatedly
- Impact: Severe performance degradation with API response times increased from 150ms to 5000ms (33x slower)
- Severity: High - Database unavailability, memory exhaustion (100%), and CPU saturation (95%) causing 75% error rate

Next Steps
1. Immediate (< 1 hour): Create/update ConfigMap 'database-config' in production namespace and restart database pod
2. Short-term (< 24 hours): 
   - Fix permissions on '/var/lib/postgresql/data' directory
   - Increase Java heap space for web-service to address OutOfMemoryErrors
   - Optimize UserService.loadAllUsers method causing memory issues
3. Long-term (< 1 week): 
   - Implement resource monitoring with alerts for CPU (>80%), memory (>90%)
   - Optimize slow database queries, particularly "SELECT * FROM users WHERE status='active'"
   - Scale up resources or implement autoscaling for web-service

Critical Alerts
- Database pod (database-pod-7b9c4d8f2a-x5m1q) in CrashLoopBackOff state
- Web-service experiencing OutOfMemoryErrors in UserService.loadAllUsers(UserService.java:45)
- Node-3 experiencing memory pressure (>85% usage)
- Web-app-deployment showing readiness probe failures with 503 errors

Troubleshooting Steps
1. Verify ConfigMap status: `kubectl get configmap database-config -n production`
2. Check database pod logs: `kubectl logs database-pod-7b9c4d8f2a-x5m1q -n production`
3. Create/update ConfigMap: `kubectl create configmap database-config --from-file=database.conf -n production`
4. Fix data directory permissions: `kubectl exec database-pod-7b9c4d8f2a-x5m1q -n production -- chmod -R 700 /var/lib/postgresql/data`
5. Restart database pod: `kubectl delete pod database-pod-7b9c4d8f2a-x5m1q -n production`
```

This investigation demonstrates how Amazon Bedrock AgentCore primitives work together:

- **Amazon Bedrock AgentCore Gateway** – Provides secure access to infrastructure APIs through MCP tools
- **Amazon Bedrock AgentCore Identity** – Handles ingress and egress authentication
- **Amazon Bedrock AgentCore Runtime** – Hosts the multi-agent solution with automatic scaling
- **Amazon Bedrock AgentCore Memory** – Personalizes Alice's experience and stores investigation knowledge for future incidents
- **Amazon Bedrock AgentCore Observability** – Captures detailed metrics and traces in CloudWatch for monitoring and debugging

The SRE agent demonstrates intelligent agent orchestration, with the supervisor routing work to specialists based on the investigation plan. The solution's memory capabilities make sure each investigation builds organizational knowledge and provides personalized experiences based on user roles and preferences.

This investigation showcases several key capabilities:

- **Multi-source correlation** – It connects database configuration issues to API performance degradation
- **Sequential investigation** – Agents work systematically through the investigation plan while providing live updates
- **Source attribution** – Findings include the specific tool and data source
- **Actionable insights** – It provides a clear timeline of events and prioritized recovery steps
- **Cascading failure detection** – It can help show how one failure propagates through the system

## Business impact

Organizations implementing AI-powered SRE assistance report significant improvements in key operational metrics. Initial investigations that previously took 30–45 minutes can now be completed in 5–10 minutes, providing SREs with comprehensive context before diving into detailed analysis. This dramatic reduction in investigation time translates directly to faster incident resolution and reduced downtime.

The solution improves how SREs interact with their infrastructure. Instead of navigating multiple dashboards and tools, engineers can ask questions in natural language and receive aggregated insights from relevant data sources. This reduction in context switching enables teams to maintain focus during critical incidents and reduces cognitive load during investigations.

Perhaps most importantly, the solution democratizes knowledge across the team. All team members can access the same comprehensive investigation techniques, reducing dependency on tribal knowledge and on-call burden. The consistent methodology provided by the solution makes sure investigation approaches remain uniform across team members and incident types, improving overall reliability and reducing the chance of missed evidence.

The automatically generated investigation reports provide valuable documentation for post-incident reviews and help teams learn from each incident, building organizational knowledge over time. Furthermore, the solution extends existing AWS infrastructure investments, working alongside services like Amazon CloudWatch, [AWS Systems Manager](https://aws.amazon.com/systems-manager/), and other AWS operational tools to provide a unified operational intelligence system.

## Extending the solution

The modular architecture makes it straightforward to extend the solution for your specific needs.

For example, you can add specialized agents for your domain:

- **Security agent** – For compliance checks and security incident response
- **Database agent** – For database-specific troubleshooting and optimization
- **Network agent** – For connectivity and infrastructure debugging

You can also replace the demo APIs with connections to your actual systems:

- **Kubernetes integration** – Connect to your cluster APIs for pod status, deployments, and events
- **Log aggregation** – Integrate with your log management service (Elasticsearch, Splunk, CloudWatch Logs)
- **Metrics platform** – Connect to your monitoring service (Prometheus, Datadog, CloudWatch Metrics)
- **Runbook repository** – Link to your operational documentation and playbooks stored in wikis, Git repositories, or knowledge bases

## Clean up

To avoid incurring future charges, use the cleanup script to remove the billable AWS resources created during the demo:

```bash
# Complete cleanup - deletes AWS resources and local files
./scripts/cleanup.sh
```

This script automatically performs the following actions:

- Stop backend servers
- Delete the gateway and its targets
- Delete Amazon Bedrock AgentCore Memory resources
- Delete the Amazon Bedrock AgentCore Runtime
- Remove generated files (gateway URIs, tokens, agent ARNs, memory IDs)

For detailed cleanup instructions, refer to [Cleanup Instructions](https://github.com/awslabs/amazon-bedrock-agentcore-samples/blob/main/02-use-cases/SRE-agent/scripts/cleanup.sh).

## Conclusion

The SRE agent demonstrates how multi-agent systems can transform incident response from a manual, time-intensive process into a time-efficient, collaborative investigation that provides SREs with the insights they need to resolve issues quickly and confidently.

By combining the enterprise-grade infrastructure of Amazon Bedrock AgentCore with standardized tool access in MCP, we've created a foundation that can adapt as your infrastructure evolves and new capabilities emerge.

The complete implementation is available in our [GitHub repository](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/02-use-cases/SRE-agent), including demo environments, configuration guides, and extension examples. We encourage you to explore the solution, customize it for your infrastructure, and share your experiences with the community.

To get started building your own SRE assistant, refer to the following resources:

- [Automate tasks in your application using AI agents](https://docs.aws.amazon.com/bedrock/latest/userguide/agents.html)
- [Amazon Bedrock AgentCore Samples GitHub repository](https://github.com/aws-samples/amazon-bedrock-agentcore-samples)
- [Model Context Protocol documentation](https://modelcontextprotocol.io/)
- [LangGraph documentation](https://langchain-ai.github.io/langgraph/)

## About the authors

![AMit Arora](/images/translated-blogs/blog1/pic3.jpg)
**Amit Arora** is an AI and ML Specialist Architect at Amazon Web Services, helping enterprise customers use cloud-based machine learning services to rapidly scale their innovations. He is also an adjunct lecturer in the MS data science and analytics program at Georgetown University in Washington, D.C.

![AMit Arora](/images/translated-blogs/blog1/pic4.jpg)
**Dheeraj Oruganty** is a Delivery Consultant at Amazon Web Services. He is passionate about building innovative Generative AI and Machine Learning solutions that drive real business impact. His expertise spans Agentic AI Evaluations, Benchmarking and Agent Orchestration, where he actively contributes to research advancing the field. He holds a master's degree in Data Science from Georgetown University. Outside of work, he enjoys geeking out on cars, motorcycles, and exploring nature.

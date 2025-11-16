---
title: "Event 3"
date: 2025-11-15
weight: 3
chapter: false
pre: " <b> 4.3 </b> "
---

## AWS Cloud Mastery Series #1

### Event Information

&emsp; **Event Name:** ​AI/ML/GenAI on AWS <br>
&emsp; **Date:** November 15, 2025 <br>
&emsp; **Speakers:** Danh Hoang Hieu Nghi (AI Engineer, Renova Cloud), Dinh Le Hoang Anh (Cloud Engineer Trainee), Lam Tuan Kiet (Senior DevOps Engineer FPT Software) <br>
&emsp; **Role:** Attendee <br>
&emsp; **Main Topic:** Building intelligent agents and generative AI applications with Amazon Bedrock <br>

### Event Overview

This workshop provided a comprehensive deep dive into Amazon Bedrock and its capabilities for building intelligent agents and generative AI applications. The session covered foundation models, prompt engineering techniques, retrieval-augmented generation (RAG), agent architecture, and practical solutions for real-world AI deployment challenges. Multiple expert speakers delivered technical presentations with live demonstrations and hands-on examples.

### Key Topics Covered

#### 1. Prompt Engineering Fundamentals

##### Prompting Techniques: Zero-Shot Prompting

![Prompting Techniques: Zero-Shot Prompting Presentation](/images/Event-Participated/pic8.jpg)

<figure>
    <figcaption>Technical presentation on Prompting Techniques focusing on Zero-Shot Prompting methods, including classification techniques and text input strategies for effective prompt engineering<figcaption>
</figure>

Essential techniques for effective model interactions:

**Core Prompting Methods:**

- **Basic Prompting**: Clear, specific instruction design
- **Chain-of-Thought (CoT) Reasoning**: Breaking down complex problems into logical steps
  - Improves accuracy for mathematical and logical reasoning
  - Increases model reliability for multi-step tasks
  - Example: "Let's think step by step..."

- **Few-Shot Learning**: Providing examples within the prompt
  - 2-3 examples typically sufficient
  - Improves task-specific performance
  - Reduces hallucinations and off-topic responses

**Best Practices:**

- Use specific, detailed instructions
- Provide context and constraints
- Include desired output format examples
- Use role-based prompting ("You are a...")

---

#### 2. Text Generation Workflow & RAG Architecture

##### Retrieval-Augmented Generation Deep Dive

![Text Generation Workflow - RAG Architecture Deep Dive](/images/Event-Participated/pic9.jpg)

<figure>
    <figcaption>Detailed explanation of Text Generation Workflow showing RAG (Retrieval-Augmented Generation) architecture components including embeddings model, vector store, semantic search, context augmentation, and data retrieval workflow<figcaption>
</figure>

**RAG Architecture Components:**

1. **Document Ingestion**: Upload and chunk knowledge base
2. **Embedding Generation**: Convert documents to vector representations
3. **Vector Storage**: Store embeddings in vector databases
4. **Query Processing**: Convert user queries to embeddings
5. **Similarity Search**: Find relevant documents from knowledge base
6. **Context Injection**: Include retrieved documents in prompt
7. **Response Generation**: Model generates answer based on context

**Knowledge Base Integration:**

- S3 buckets for document storage
- OpenSearch, Pinecone, or Vector databases for embeddings
- Lambda for processing and indexing
- CloudWatch for monitoring knowledge base quality

**Advantages:**

- Current information without model retraining
- Cited sources and fact verification
- Reduced hallucinations
- Domain-specific knowledge integration

---

#### 3. Amazon Bedrock AgentCore Architecture

##### Comprehensive Agentic Platform Components

![Amazon Bedrock AgentCore - Component Architecture](/images/Event-Participated/pic17.jpg)

<figure>
    <figcaption>Amazon Bedrock AgentCore - Comprehensive agentic platform showing core components: Runtime, Memory, Identity, Gateway, Code Interpreter, Browser Tool, and Observability for production-ready agent deployment<figcaption>
</figure>

**Key Architecture Components:**

- **Runtime**: Core execution environment for agent operations
- **Memory**: State and context management for agents
- **Identity**: Authentication and authorization framework
- **Gateway**: API gateway for agent communication
- **Code Interpreter**: Execute and validate generated code
- **Browser Tool**: Web interaction and data retrieval capabilities
- **Observability**: Monitoring, logging, and tracing for agents

---

#### 4. AgentCore Services & Scaling

##### Services Enabling Agents at Scale

![AgentCore Services - Architecture for Scale](/images/Event-Participated/pic18.jpg)

<figure>
    <figcaption>AgentCore services architecture showing how agents scale with Framework, Agent Instruction, Agent local tools, Agent context, AgentCore Gateway, Browser, Code Interpreter, Identity, Memory, and Observability components working together<figcaption>
</figure>

**Scalable Agent Architecture:**

**Client Layer:**
- Multiple client connections
- Request routing and load balancing

**Agent Runtime Layer:**
- Framework: Core agent execution engine
- Agent Instruction: Task definitions and workflows
- Agent local tools: Built-in and custom tools
- Agent context: State management and memory

**AgentCore Services:**
- Gateway: Request processing and routing
- Browser Tool: Web interaction and scraping
- Code Interpreter: Safe code execution
- Identity: Permission and access control
- Memory: Persistent state management
- Observability: Full tracing and monitoring

---

#### 5. From Prototype to Production

##### Production Challenges & Solutions

![Prototype to Production - The Chasm](/images/Event-Participated/pic19.jpg)

<figure>
    <figcaption>The prototype to production "chasm" illustrating the challenges of moving from POC (Proof of Concept) to production AI agents, including Performance, Scalability, Security, Governance, and achieving meaningful business value<figcaption>
</figure>

**Production Readiness Checklist:**

**Performance:**
- Latency optimization
- Throughput requirements
- Resource efficiency

**Scalability:**
- Concurrent agent handling
- Load distribution
- Auto-scaling capabilities

**Security:**
- Access control and authentication
- Data encryption and protection
- Audit logging and compliance

**Governance:**
- Model governance frameworks
- Decision traceability
- Regulatory compliance

**Business Value:**
- ROI measurement
- Use case validation
- Integration with business processes

---

#### 6. Retrieval-Augmented Generation API

##### Knowledge Bases for Amazon Bedrock

![RetrieveAndGenerate API - Knowledge Bases Implementation](/images/Event-Participated/pic20.jpg)

<figure>
    <figcaption>RetrieveAndGenerate API for Knowledge Bases showing the complete flow: user input, query generation with embeddings, document retrieval, context augmentation, and response generation from LLM<figcaption>
</figure>

**RAG Implementation Flow:**

**Step 1: Query Preparation**
- User input processing
- Generate query embedding from user input

**Step 2: Knowledge Base Retrieval**
- Retrieve similar documents from knowledge bases
- Semantic matching using embeddings

**Step 3: Context Augmentation**
- Augment user query with retrieved documents
- Create enhanced prompt with relevant context

**Step 4: Response Generation**
- Generate response from LLM using augmented context
- Return answer with proper citations

**Benefits:**
- Grounded responses with source attribution
- Current information without retraining
- Reduced hallucinations
- Cost-effective knowledge updates

---

#### 7. Frameworks for Building Agents

##### Open Source Agent Frameworks

![Frameworks for Building Agents - Ecosystem Overview](/images/Event-Participated/pic21.jpg)

<figure>
    <figcaption>Comprehensive overview of open source frameworks and SDKs for building agents including Strands, LangGraph, LangChain, Crew.ai, Google ADK, LlamaIndex, and more<figcaption>
</figure>

**Popular Agent Frameworks:**

**Strands SDK**
- Agent orchestration framework
- Multi-step workflow support

**LangGraph & LangChain**
- Language model chaining and orchestration
- Modular component architecture
- Extensive integration ecosystem

**Crew.ai**
- Multi-agent collaboration framework
- Role-based agent assignment
- Task dependency management

**Google ADK (Agent Development Kit)**
- Google Cloud native agent framework
- Gemini integration

**LlamaIndex**
- Data indexing and retrieval framework
- RAG pipeline optimization
- Knowledge base management

**OpenAI SDKs**
- Official OpenAI agent frameworks
- Function calling and tool use

---

#### 8. AWS AI Services Ecosystem

##### Comprehensive Suite of AI/ML Services

![AWS AI Services - Comprehensive Service Portfolio](/images/Event-Participated/pic22.jpg)

<figure>
    <figcaption>AWS AI services ecosystem including Rekognition (vision), Translate (language), Textract (document), Transcribe (speech-to-text), Polly (text-to-speech), Comprehend (NLP), Kendra (search), Lookout (anomaly), and Personalize (recommendations)<figcaption>
</figure>

**Core AI/ML Services:**

**Vision Services:**
- **Amazon Rekognition**: Image and video analysis, object detection, face recognition

**Language Services:**
- **Amazon Translate**: Neural machine translation
- **Amazon Transcribe**: Automatic speech recognition
- **Amazon Comprehend**: Natural language processing and sentiment analysis
- **Amazon Polly**: Text-to-speech synthesis

**Data & Knowledge:**
- **Amazon Textract**: Extract text and data from documents
- **Amazon Kendra**: Intelligent document search and retrieval

**Intelligence Services:**
- **Amazon Lookout**: Anomaly detection in metrics and time-series data
- **Amazon Personalize**: Real-time personalization engine
- **Amazon Forecast**: Time-series forecasting

---

#### 9. Speaker Presentations & Technical Deep Dives

##### Amazon Bedrock AgentCore Opening Keynote

![Amazon Bedrock AgentCore Opening - Danh Hoang Hieu Nghi](/images/Event-Participated/pic13.jpg)

<figure>
    <figcaption>Opening keynote on Amazon Bedrock AgentCore presented by Danh Hoang Hieu Nghi (AI Engineer, Renova Cloud), introducing core concepts of intelligent agent architecture and multi-step workflow orchestration<figcaption>
</figure>

**Keynote Topics:**
- Amazon Bedrock capabilities and vision
- AgentCore architecture fundamentals
- Real-world use cases for intelligent agents
- Getting started with Bedrock Agents

---

##### AWS AI Services Overview

![AWS AI Services Presentation - Other Pretrained AI Services](/images/Event-Participated/pic14.jpg)

<figure>
    <figcaption>Technical presentation on Other Pretrained AI Services including Rekognition, Polly, Transcribe, and Comprehend, showcasing AWS's comprehensive AI/ML service ecosystem for real-world applications<figcaption>
</figure>

**Services Covered:**
- Amazon Rekognition for computer vision
- Amazon Polly for speech synthesis
- Amazon Transcribe for speech-to-text
- Amazon Comprehend for NLP and sentiment analysis
- Real-world use case demonstrations

---

##### Generative AI with Amazon Bedrock - Multi-Speaker Panel

![Multi-Speaker Panel - Generative AI with Amazon Bedrock](/images/Event-Participated/pic15.jpg)

<figure>
    <figcaption>Multi-speaker panel discussion on Generative AI with Amazon Bedrock featuring Lam Tuan Kiet (Senior DevOps Engineer FPT Software), Dinh Le Hoang Anh (Cloud Engineer Trainee), and other speakers discussing practical implementations and best practices<figcaption>
</figure>

**Panel Discussion Topics:**
- Foundation model selection and comparison
- Prompt engineering best practices
- RAG implementation strategies
- Production deployment considerations
- Cost optimization for AI workloads

---

##### Workshop Team Photo

![Workshop Team Photo - AWS Cloud Mastery Event](/images/Event-Participated/pic16.jpg)

<figure>
    <figcaption>Group photo of workshop participants and speakers in front of the Kahoot interactive display, representing the collaborative learning environment and successful completion of the AWS Cloud Mastery Series #1 event<figcaption>
</figure>

---

#### 10. Workshop Engagement & Participants

##### Workshop Sessions & Interactive Learning

![Technical Workshop Session](/images/Event-Participated/pic10.jpg)

<figure>
    <figcaption>Workshop presentation on advanced AI/ML techniques and AWS service integration<figcaption>
</figure>

![Live Technical Demonstrations](/images/Event-Participated/pic11.jpg)

<figure>
    <figcaption>Attendees engaged in technical presentations and learning about AWS AI services<figcaption>
</figure>

![Workshop Community & Knowledge Sharing](/images/Event-Participated/pic12.jpg)

<figure>
    <figcaption>Workshop participants engaging in discussion and knowledge exchange during the AWS Cloud Mastery event<figcaption>
</figure>

---

### Q&A Session: Technical Challenges & Solutions

#### Q1: Managing Large Messages Without SQS

**Problem Statement:**
Handling high-volume message processing and large payloads without using SQS.

**Solutions Discussed:**

1. **Direct Invocation with Concurrency Limits**
   - Use Lambda reserved concurrency
   - Implement application-level rate limiting
   - Use DynamoDB for in-flight request tracking
   - Pros: Simple, cost-effective for moderate loads
   - Cons: Limited throughput, no built-in retry mechanism

2. **DynamoDB Streams**
   - Write messages to DynamoDB
   - Lambda triggered by DynamoDB Streams
   - Provides ordering guarantees and replay capabilities
   - Pros: Durability, batch processing
   - Cons: Storage costs, higher latency

3. **Kinesis Data Streams**
   - Stream large volumes of messages
   - Shard-based scaling
   - Consumer groups for parallel processing
   - Better alternative to SQS for high-volume scenarios
   - Pros: High throughput, real-time processing, replay
   - Cons: Higher cost, more complex setup

4. **S3 + Event Notifications**
   - Large payloads to S3
   - S3 events trigger processing
   - Works well for batch jobs
   - Pros: Unlimited payload size, durable storage
   - Cons: Higher latency, eventual consistency

**Recommendation:**
For most cases, **Kinesis** offers the best balance of throughput, features, and cost. For simple batch processing, **S3 + Lambda** is sufficient.

---

#### Q2: Rate Limiting AI Chatbots (1-2 requests/minute limit)

**Problem Statement:**
API or model constraints limiting chatbot to 1-2 requests per minute, needing solutions to handle higher user demand.

**Solutions Discussed:**

1. **Request Queuing & Batch Processing**
   - Queue incoming requests in SQS/Kinesis
   - Process in batches at allowed rate
   - Send completion notifications via SNS/EventBridge
   - Provide polling endpoint for users to check status
   - Pros: Fair queuing, backpressure handling
   - Cons: Higher latency for end users

2. **Response Caching**
   - Cache common questions and responses
   - Check cache before making API call
   - Use DynamoDB or ElastiCache for caching
   - TTL-based cache invalidation
   - Pros: Significantly reduces API calls
   - Cons: Stale data risk, cache management overhead

3. **Session-Based Concurrency Control**
   - Implement per-user or per-session rate limits
   - Track concurrent sessions in DynamoDB
   - Queue requests from same session sequentially
   - Pros: Prevents single user from dominating capacity
   - Cons: Complex state management

4. **Multiple Model Instances / Load Balancing**
   - If using on-premises or containerized models
   - Run multiple instances behind load balancer
   - Distribute requests across instances
   - Pros: Increased throughput
   - Cons: Infrastructure overhead

5. **Request Priority & Prioritization Queue**
   - Implement priority levels (VIP users, critical requests)
   - SQS with message priority attributes
   - Process high-priority requests first
   - Pros: SLA guarantees for important users
   - Cons: Complexity in fairness logic

6. **Async Processing with Callbacks**
   - Accept request and return job ID immediately
   - Process asynchronously (0-5 minute wait)
   - Notify user via webhook/email when ready
   - WebSocket for real-time updates
   - Pros: Better UX, respects API limits
   - Cons: Requires async architecture changes

**Recommended Architecture for AI Chatbot:**

```
User Request → API Gateway
       ↓
   [Check Cache] → Return if found
       ↓
   [SQS Queue] → Enforce rate limit (1-2 req/min)
       ↓
   [Lambda] → Call Bedrock API
       ↓
   [DynamoDB] → Cache result
       ↓
   [SNS] → Notify user (email/webhook)
```

**Key Metrics to Monitor:**
- Queue depth
- API latency
- Cache hit rate
- End-to-end user latency
- API call throttling events

---

### Key Takeaways and Outcomes

**Technical Knowledge Gained:**

- Deep understanding of Amazon Bedrock architecture and capabilities
- Foundation model selection and comparison
- Advanced prompt engineering techniques (CoT, Few-shot, Zero-shot)
- RAG implementation patterns for knowledge integration
- Bedrock Agents architecture and multi-step workflow automation
- AgentCore components and scalability strategies
- Framework ecosystem for agent development
- AWS AI service ecosystem and real-world use cases

**Practical Solutions:**

- Effective rate limiting and message handling patterns for AI applications
- Caching strategies to optimize API usage
- Queue-based architectures for handling bottlenecks
- Production readiness checklist for AI agents
- Real-world examples of AI service integration

**Architecture Patterns Learned:**

- Serverless chatbot architecture
- RAG pipeline design and implementation
- Agent-based automation workflows
- Safe and compliant AI implementation with guardrails
- Scalable multi-agent systems

### Personal Reflections

This workshop significantly deepened my understanding of building production-ready generative AI applications on AWS. The combination of foundation model knowledge, advanced prompting techniques, comprehensive architecture diagrams, and practical solutions provides a solid foundation for developing intelligent agents and AI-powered applications at scale.

The workshop covered both theoretical concepts and practical implementations, with real-world examples showing how to overcome common challenges in AI deployment. The multi-speaker format allowed for diverse perspectives on best practices and emerging patterns in AI development.

Key areas for further exploration:

- Advanced RAG with hybrid search (semantic + keyword)
- Fine-tuning foundation models for specialized domains
- Multi-agent systems and agent collaboration patterns
- MLOps and monitoring for GenAI applications
- Cost optimization for high-volume AI workloads
- Integration of multiple AWS AI services in complex workflows

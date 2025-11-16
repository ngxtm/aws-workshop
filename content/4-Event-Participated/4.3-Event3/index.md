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
&emsp; **Speaker:** Danh Hoang Hieu Nghi (AI Engineer, Renova Cloud), Dinh Le Hoang Anh (Cloud Engineer Trainee), Lam Tuan Kiet (Senior DevOps Engineer FPT Software) <br>
&emsp; **Role:** Attendee <br>
&emsp; **Main Topic:** Building intelligent agents and generative AI applications with Amazon Bedrock <br>

### Event Overview

This workshop provided a deep dive into Amazon Bedrock and its capabilities for building intelligent agents and generative AI applications. The session covered foundation models, prompt engineering techniques, retrieval-augmented generation (RAG), and practical implementations of Bedrock agents with real-world examples.

### Key Topics Covered

#### 1. Amazon Bedrock Agent Core

Amazon Bedrock Agents enable autonomous multi-step workflows and tool integrations, allowing AI to make decisions and take actions without human intervention.

**Key Capabilities:**

- Multi-step workflow orchestration
- Tool integration and function calling
- Autonomous decision-making based on context
- Real-time API and data source integration
- Agent memory and context management

**Architecture Highlights:**

- Event-driven agent workflows
- Stateful conversation management
- Built-in error handling and retry mechanisms
- Support for tool chains and conditional logic

---

#### 2. Generative AI with Amazon Bedrock

##### 2.1 Foundation Models: Selection & Comparison

Foundation models available on AWS:

- **Claude** (by Anthropic) - Advanced reasoning and long-context understanding
- **Llama** (by Meta) - Open-source, flexible, cost-effective
- **Titan** (by AWS) - Optimized for AWS ecosystem, multilingual support

**Selection Guide:**

- **Claude**: Best for complex reasoning, nuanced text generation, and long documents
- **Llama**: Ideal for cost-sensitive deployments and privacy-first applications
- **Titan**: Recommended for AWS-native applications and embeddings

##### 2.2 Prompt Engineering

Essential techniques for effective model interactions:

**Techniques:**

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

##### 2.3 Retrieval-Augmented Generation (RAG)

Architecture for enhancing AI with external knowledge without model fine-tuning.

**RAG Architecture:**

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

##### 2.4 Bedrock Agents: Practical Implementation

Building multi-step workflows with tool integrations.

**Key Components:**

- Agent brain (foundation model)
- Action groups (available tools/APIs)
- Knowledge bases (RAG documents)
- Prompts and instructions
- Session management

**Use Cases:**

- Intelligent customer service assistants
- Document analysis and summarization workflows
- Code generation and debugging agents
- Data analysis and reporting automation

##### 2.5 Guardrails: Safety & Content Filtering

Implementing safety measures for production AI applications.

**Guardrails Features:**

- Content filtering (profanity, explicit content)
- PII (Personally Identifiable Information) detection and redaction
- Harmful content blocking
- Sensitive topic awareness
- Custom rule definition

**Implementation:**

- Inline guardrails in model invocations
- Pre and post-processing filtering
- Audit logging for safety events
- Compliance tracking

##### 2.6 Live Demo: Building a Generative AI Chatbot

Walk-through of building a production-ready chatbot:

**Architecture:**

- Frontend: Chat interface (web/mobile)
- API Gateway: Route requests to backend
- Lambda: Orchestrate Bedrock API calls
- RDS/DynamoDB: Store conversation history and user data
- CloudWatch: Monitor and log interactions
- VectorDB: Store RAG knowledge base

**Key Implementation Steps:**

1. Set up IAM roles and Bedrock access
2. Create model invocation API
3. Implement conversation memory management
4. Integrate RAG with knowledge base
5. Add error handling and rate limiting
6. Deploy with scalability in mind

---

#### 3. AWS AI Services Ecosystem

##### 3.1 Amazon Rekognition

**Purpose:** Computer vision service for image and video analysis

**Capabilities:**

- Object and scene detection
- Face recognition and analysis
- Text detection in images (OCR)
- Celebrity recognition
- Activity detection in videos

**Real-World Use Cases:**

- Content moderation for social media platforms
- Security and surveillance systems (person re-identification)
- Accessibility features (image description generation)
- Retail analytics (shelf inventory tracking)
- Medical imaging analysis (with custom models)

---

##### 3.2 Amazon Polly

**Purpose:** Text-to-speech service

**Features:**

- 140+ voices across 30+ languages
- Neural TTS for human-like quality
- SSML markup for fine-grained control
- Custom pronunciation lexicons

**Real-World Use Cases:**

- Audiobook and podcast generation
- Accessibility features for vision-impaired users
- Interactive voice response (IVR) systems
- E-learning platform narration
- Game character dialogue generation

---

##### 3.3 Amazon Transcribe

**Purpose:** Automatic speech recognition (ASR)

**Features:**

- Real-time and batch transcription
- Automatic punctuation
- Speaker identification
- Medical terminology support
- Multi-language support

**Real-World Use Cases:**

- Meeting and call transcription (Teams, Zoom integration)
- Compliance recording for financial institutions
- Customer service quality assurance
- Legal and medical documentation
- Content accessibility (video subtitles)

---

##### 3.4 Amazon Comprehend

**Purpose:** Natural language processing and text analysis

**Capabilities:**

- Sentiment analysis
- Entity recognition (people, places, organizations)
- Key phrase extraction
- Language detection
- Topic modeling

**Real-World Use Cases:**

- Social media sentiment monitoring
- Customer feedback analysis
- Resume and CV screening (entity extraction)
- Content classification and tagging
- Fraud detection (linguistic patterns)

---

##### 3.5 Other AI Services

**Amazon Lookout for Anomalies:** Detect unusual patterns in metrics and time-series data
**Amazon Forecast:** Time-series forecasting for demand planning
**Amazon Personalize:** Real-time personalization engine for recommendations
**Amazon Textract:** Extract text and data from documents
**Amazon Lookout for Vision:** Detect defects in manufacturing/quality control

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
- Advanced prompt engineering techniques (CoT, Few-shot)
- RAG implementation patterns for knowledge integration
- Bedrock Agents for multi-step workflow automation
- AWS AI service ecosystem and use cases

**Practical Solutions:**

- Effective rate limiting and message handling patterns for AI applications
- Caching strategies to optimize API usage
- Queue-based architectures for handling bottlenecks
- Real-world examples of AI service integration

**Architecture Patterns Learned:**

- Serverless chatbot architecture
- RAG pipeline design
- Agent-based automation workflows
- Safe and compliant AI implementation with guardrails

### Personal Reflections

This workshop significantly deepened my understanding of building production-ready generative AI applications on AWS. The combination of foundation model knowledge, advanced prompting techniques, and practical architectural patterns provides a solid foundation for developing intelligent agents and AI-powered applications.

Key areas for further exploration:

- Advanced RAG with hybrid search (semantic + keyword)
- Fine-tuning foundation models for specialized domains
- Multi-agent systems and agent collaboration
- MLOps and monitoring for GenAI applications
- Cost optimization for high-volume AI workloads

### Event Photos

#### Prompt Engineering Techniques & Zero-Shot Prompting

![Prompting Techniques: Zero-Shot Prompting Presentation](/images/Event-Participated/pic8.jpg)

<figure>
    <figcaption>Technical presentation on Prompting Techniques focusing on Zero-Shot Prompting methods, including classification techniques and text input strategies for effective prompt engineering<figcaption>
</figure>

#### Text Generation Workflow & RAG Architecture

![Text Generation Workflow - RAG Architecture Deep Dive](/images/Event-Participated/pic9.jpg)

<figure>
    <figcaption>Detailed explanation of Text Generation Workflow showing RAG (Retrieval-Augmented Generation) architecture components including embeddings model, vector store, semantic search, context augmentation, and data retrieval workflow<figcaption>
</figure>

#### Additional Technical Sessions

![Workshop Session - Additional Technical Content](/images/Event-Participated/pic10.jpg)

<figure>
    <figcaption>Workshop presentation on advanced AI/ML techniques and AWS service integration<figcaption>
</figure>

![Workshop Environment - Live Technical Demonstrations](/images/Event-Participated/pic11.jpg)

<figure>
    <figcaption>Attendees engaged in technical presentations and learning about AWS AI services<figcaption>
</figure>

![Workshop Community & Knowledge Sharing](/images/Event-Participated/pic12.jpg)

<figure>
    <figcaption>Workshop participants engaging in discussion and knowledge exchange during the AWS Cloud Mastery event<figcaption>
</figure>

#### Speaker Presentations & Expert Sessions

![Amazon Bedrock AgentCore Opening Presentation by Danh Hoang Hieu Nghi](/images/Event-Participated/pic13.jpg)

<figure>
    <figcaption>Opening keynote on Amazon Bedrock AgentCore presented by Danh Hoang Hieu Nghi (AI Engineer, Renova Cloud), introducing core concepts of intelligent agent architecture and multi-step workflow orchestration<figcaption>
</figure>

![AWS AI Services Overview - Other Pretrained AI Services](/images/Event-Participated/pic14.jpg)

<figure>
    <figcaption>Technical presentation on Other Pretrained AI Services including Rekognition, Polly, Transcribe, and Comprehend, showcasing AWS's comprehensive AI/ML service ecosystem for real-world applications<figcaption>
</figure>

![Generative AI with Amazon Bedrock - Multi-Speaker Panel Discussion](/images/Event-Participated/pic15.jpg)

<figure>
    <figcaption>Multi-speaker panel discussion on Generative AI with Amazon Bedrock featuring Lam Tuan Kiet (Senior DevOps Engineer FPT Software), Dinh Le Hoang Anh (Cloud Engineer Trainee), and other speakers discussing practical implementations and best practices<figcaption>
</figure>

![Workshop Team Photo - AWS Cloud Mastery Event Conclusion](/images/Event-Participated/pic16.jpg)

<figure>
    <figcaption>Group photo of workshop participants and speakers in front of the Kahoot interactive display, representing the collaborative learning environment and successful completion of the AWS Cloud Mastery Series #1 event<figcaption>
</figure>
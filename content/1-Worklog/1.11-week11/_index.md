---
title: "Week 11 Worklog"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

### Week 11 Objectives:
- Develop backend - RAG System (combining hybrid search engine and LLM)
- Practice Lab 26-29: Serverless, SAM, SageMaker, AWS Toolkit
- Research and implement RAG for MapVibe chatbot

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - Team project meeting: Develop backend - RAG System<br>&emsp; + Improve accuracy of hybrid search engine with LLM<br>&emsp; + Add chat feature for users to search<br>&emsp; + Research best practices for RAG systems | 17/11/2025 | 17/11/2025 | [RAG Paper](https://arxiv.org/pdf/2005.11401) |
| 3 | - Practice Lab 26: Serverless - Front-end calling API Gateway<br>&emsp; + Deploy Front-end, setup API Gateway<br>&emsp; + Test API with Postman and Front-end<br>- Practice Lab 27: Serverless with Lambda, API Gateway and SAM<br>&emsp; + Deploy Frontend and Backend<br>&emsp; + Deploy ride times system<br>&emsp; + Image processing (Chroma Key, Photo-Compositing) | 18/11/2025 | 18/11/2025 | [Lab 26](https://000135.awsstudygroup.com/vi/)<br>[Lab 27](https://000066.awsstudygroup.com/vi/) |
| 4 | - Practice Lab 28: SageMaker Immersion Day Workshop<br>&emsp; + Create SageMaker Studio, clone GitHub repo<br>&emsp; + Feature Engineering: Data Wrangler, analyze Dataset<br>&emsp; + Train/Tune/Deploy XGBoost model<br>&emsp; + Evaluate performance and auto-tune model | 19/11/2025 | 19/11/2025 | [Lab 28](https://000200.awsstudygroup.com/vi/) |
| 5 | - Team project meeting: Develop backend - RAG System<br>&emsp; + Run and test RAG system with collected data<br>&emsp; + Complete RAG system | 20/11/2025 | 20/11/2025 | [AWS RAG](https://aws.amazon.com/vi/what-is/retrieval-augmented-generation/) |
| 6 | - Practice Lab 29: AWS Toolkit for VS Code - Amazon Q & CodeWhisperer<br>&emsp; + Install AWS Toolkit for VS Code<br>&emsp; + Connect AWS account, change Region<br>&emsp; + Authentication: IAM Identity Center, IAM credentials, Builder ID<br>&emsp; + Interact with services: AWS Explorer, Amazon Q, CodeWhisperer | 21/11/2025 | 21/11/2025 | [Lab 29](https://000087.awsstudygroup.com/vi/) |

### Week 11 Achievements:

#### **Team Project - RAG System Development**
- **Daily Meeting (14/11)**: Review hybrid search, plan RAG integration
  - Hybrid search đã work, giờ cần wrap với LLM để trả lời natural language
  - Phú + Quân phụ trách RAG implementation
- **RAG Architecture Design**:
  ```
  User Query → Query Understanding → Hybrid Search (Top-K) → Context Building → LLM Generation → Response
  ```
  - **Naive RAG problem**: Stuff all context vào prompt → exceed token limit, expensive
  - **Solution**: Retrieve top-5 results, summarize context, generate focused response
- **Implementation Details**:
  - Query understanding: Extract location, cuisine type, preferences từ natural language
  - Context template:
    ```
    Dựa trên thông tin sau về các quán ăn:
    {retrieved_places}
    
    Hãy trả lời câu hỏi: {user_query}
    ```
  - LLM: Claude 3 Haiku (fast, cheap) cho chat, Sonnet cho complex queries
- **Chat Features**:
  - Conversation history: Store trong session, pass to LLM for context
  - Multi-turn: "Còn quán nào gần hơn không?" → LLM hiểu context từ previous turns
  - **Fallback**: Khi không tìm được results → "Xin lỗi, tôi không tìm thấy quán phù hợp..."
- **Testing**:
  - "Tìm quán cafe có wifi quận 3" → Return relevant cafes với wifi mention
  - "Quán đó có chỗ đậu xe không?" → LLM check context của quán vừa mention
  - **Latency**: ~2s end-to-end (search 100ms + LLM 1.5s + overhead)

#### **Lab 26 & 27: Serverless Applications**
- **Frontend Deployment**:
  - S3 bucket với Static Website Hosting enabled
  - CloudFront distribution cho caching + HTTPS
  - **Tip**: Invalidate cache sau mỗi deploy: `aws cloudfront create-invalidation --distribution-id XXX --paths "/*"`
- **SAM Deep Dive**:
  - `template.yaml`: Define Lambda, API Gateway, DynamoDB trong 1 file
  - `samconfig.toml`: Store deployment parameters (stack name, region, S3 bucket)
  - **Local testing**: `sam local start-api` → Test API locally trước deploy
- **Image Processing (Chroma Key)**:
  - Remove background từ image uploaded
  - Composite với new background
  - **Use case**: Product photos, profile pictures
  - Lambda Layer cho sharp/jimp libraries

#### **Lab 28: SageMaker ML Workshop**
- **SageMaker Studio**:
  - JupyterLab environment managed by AWS
  - Kernel: Python 3 (Data Science), có pre-installed ML libraries
  - **Gotcha**: Studio domain phải cùng VPC với data sources
- **Data Wrangler**:
  - Visual data transformation: Join, filter, encode, scale
  - Export to: Processing Job, Pipeline, Feature Store
  - **Benefit**: Non-ML engineers có thể prepare data, democratize ML
- **XGBoost Training**:
  - Built-in algorithm, không cần write training code
  - Hyperparameters: `max_depth`, `eta`, `num_round`, `objective`
  - **Automatic Model Tuning**: Define ranges, SageMaker tìm best params
  - Training job: ml.m5.xlarge, ~15 min cho small dataset
- **Model Deployment**:
  - Real-time endpoint: Always-on, ~$0.05/hour cho ml.t3.medium
  - Serverless inference: Pay per request, cold start ~5s
  - **Choice**: Serverless cho MapVibe (low traffic, cost-sensitive)

#### **Lab 29: AWS Toolkit & Amazon Q**
- **AWS Toolkit trong VS Code**:
  - Explorer: Browse S3, Lambda, CloudWatch directly từ IDE
  - Deploy Lambda: Right-click → Deploy to AWS
  - **Time saver**: Không cần switch giữa IDE và Console
- **Amazon Q Developer**:
  - Inline suggestions khi type code
  - `/explain`: Giải thích code block
  - `/fix`: Suggest fixes cho errors
  - `/optimize`: Performance improvements
  - **Security scanning**: Flag potential vulnerabilities trong code
- **Authentication Methods**:
  - **IAM Identity Center** (recommended): SSO, centralized management
  - **IAM credentials**: Access key + secret, store trong `~/.aws/credentials`
  - **Builder ID**: Free tier, limited features, good for personal projects

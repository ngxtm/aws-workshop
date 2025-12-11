---
title: "Week 11 Worklog"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

### Week 11 Objectives:
- **Develop Core RAG System**: Handle Embeddings and Vector Search
- **Build Database**: Implement PostgreSQL with pgvector
- **API Development**: Basic CRUD for Reviews

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - **Local Database Setup**: Setup PostgreSQL docker<br>- Configure pgvector extension locally | 17/11/2025 | 17/11/2025 | |
| 3 | - **RAG Implementation (Local)**:<br>&emsp; + Write script to convert text to vector (Embeddings)<br>&emsp; + Test search similarity on local DB | 18/11/2025 | 18/11/2025 | |
| 4 | - **Backend API**: Develop Create Review, Get Review List APIs<br>- Integrate RAG logic into review creation flow (auto vectorize on save) | 19/11/2025 | 19/11/2025 | |
| 5 | - **Frontend Base UI**: General Layout, Header, Footer<br>- Connect Get Review API to display basic UI | 20/11/2025 | 20/11/2025 | |
| 6 | - **Refine RAG**: Optimize prompt for LLM (Local model/OpenAI test) to answer questions based on retrieved context | 21/11/2025 | 21/11/2025 | |

### Week 11 Achievements:
- **Core Feature - RAG**: Successfully ran basic RAG flow Locally. System can retrieve relevant context from vector DB.
- **API Foundation**: Basic CRUD APIs are stable.
- **Frontend-Backend Integration**: Successfully connected FE and BE for listing screens.
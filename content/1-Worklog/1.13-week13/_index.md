---
title: "Week 13 Worklog"
date: 2025-12-01
weight: 13
chapter: false
pre: " <b> 1.13 </b> "
---

### Week 13 Objectives:
- **Complete Search & Detail Feature**: For Admin and User
- **Develop Summary Review**: Use AI to summarize reviews
- **Integration**: Connect all key features

### Tasks to complete this week:

| Day | Task | Start Date | Completion Date | References |
|---|---|---|---|---|
| 2 | - **Backend - Search API**:<br>&emsp; + Implement Advanced Search with filters<br>&emsp; + Optimize query performance | 01/12/2025 | 01/12/2025 |  |
| 3 | - **Frontend - UI Implementation**:<br>&emsp; + Admin Search Page (Table, Filters, Pagination)<br>&emsp; + Resource Detail Page (Info, History, Status) | 02/12/2025 | 02/12/2025 |  |
| 4 | - **Feature - Summary Review (Backend)**:<br>&emsp; + Develop review aggregation API<br>&emsp; + Integrate RAG/Bedrock to generate summary content | 03/12/2025 | 03/12/2025 |  |
| 5 | - **Feature - Summary Review (Frontend)**:<br>&emsp; + Display review summary on UI<br>&emsp; + Visualize rating charts | 04/12/2025 | 04/12/2025 |  |
| 6 | - **Code Merge & Integration**:<br>&emsp; + Merge `feature/search-and-detail` and `features/summary-review`<br>&emsp; + Resolve conflicts, ensure successful build | 05/12/2025 | 05/12/2025 |  |

### Week 13 Achievements:

#### **Feature Completion**
- **Search & Detail**: 100% completed search and detail view features for both Admin and End-users. Smooth UI, good UX.
- **Summary Review AI**:
  - The project's "killer feature" is working. AI can read multiple reviews and summarize key points (Pros/Cons) accurately.
  - Summary generation speed improved due to Lambda optimization.

#### **System Integration**
- All modules (Auth, RAG, Search, Review) are connected.
- Data flows smoothly from User -> API -> DB -> AI -> User.
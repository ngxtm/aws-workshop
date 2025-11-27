---
title: "Week 11 Worklog"
date: 2025-11-17
weight: 11
chapter: false
pre: " <b> 1.11 </b> "
---

### Week 11 Objective: MapVibe Project Kickoff

After 10 weeks of foundational knowledge, it's time to start building the actual MapVibe project. This week focuses on environment setup and project structure.

### Tasks to complete this week:

| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | References                             |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | -------------------------------------- |
| Mon | - **Requirements & Architecture Analysis**:<br>&emsp; + Review `TECH_STACK.md` and `ARCHITECTURE_DECISIONS.md`<br>&emsp; + Redraw Data Flow<br>&emsp; + Finalize technologies (Bun, Vite, Lambda, RDS) | 17/11/2025 | 17/11/2025      | [MapVibe Docs](../../docs/)            |
| Tue | - **Setup Monorepo**:<br>&emsp; + Initialize project with Turborepo<br>&emsp; + Configure Bun Workspaces<br>&emsp; + Create folder structure: `apps/web`, `packages/ui`, `packages/db`                 | 18/11/2025 | 18/11/2025      | [Turborepo Docs](https://turbo.build/) |
| Wed | - **Frontend Setup**:<br>&emsp; + Init React 19 + Vite in `apps/web`<br>&emsp; + Install Tailwind CSS v3<br>&emsp; + Setup basic UI Component Library                                                  | 19/11/2025 | 19/11/2025      |                                        |
| Thu | - **Backend & Database Setup**:<br>&emsp; + Configure Kysely for Database migrations<br>&emsp; + Write first migration (User table)<br>&emsp; + Setup Docker Compose for local Postgres                | 20/11/2025 | 20/11/2025      |                                        |
| Fri | - **CI/CD Initial Setup**:<br>&emsp; + Create repo on GitLab<br>&emsp; + Write basic `.gitlab-ci.yml` (Install & Build)<br>&emsp; + Push first code to repo                                            | 21/11/2025 | 21/11/2025      |                                        |
| Sat | - **Review & Planning**:<br>&emsp; + Plan details for Sprint 1 (Next week)<br>&emsp; + Assign specific tasks                                                                                           | 22/11/2025 | 22/11/2025      |                                        |
| Sun | - Rest & Prepare mindset for coding                                                                                                                                                                    | 23/11/2025 | 23/11/2025      |                                        |

### Week 11 Achievements:

- Have a complete Project Skeleton running locally.
- Green CI/CD Pipeline (Pass).
- Ready to code features next week.

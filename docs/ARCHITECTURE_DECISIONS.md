# Architecture Decision Records (ADR)

> Documentation of architectural decisions made throughout MapVibe development

## Format
Each ADR includes:
- **Date**: When the decision was made
- **Status**: Proposed, Accepted, Deprecated, Superseded
- **Context**: Why we needed to make this decision
- **Decision**: What we decided
- **Consequences**: Trade-offs and implications

---

## ADR-001: PostgreSQL with PostGIS for Geospatial Data

**Date**: 2024-11-06
**Status**: Accepted

### Context
MapVibe requires efficient geospatial queries for "restaurants near me" functionality with radius-based search. Need to support:
- Find restaurants within X km of a point
- Order by distance
- Filter by category/rating while maintaining geo performance

### Decision
Use PostgreSQL 15+ with PostGIS extension on AWS RDS instead of DynamoDB with geohashing or dedicated geospatial databases.

### Consequences

**Positive**:
- Native GIS support with `ST_DWithin` and distance calculations
- Mature ecosystem with excellent documentation
- Can combine geospatial + full-text + filtering in single query
- AWS RDS provides managed backups, replication, monitoring

**Negative**:
- Higher cost than DynamoDB (~$70-100/month vs ~$10-20/month)
- Requires RDS Proxy for Lambda connection pooling
- Vertical scaling limits (though sufficient for MVP)

**Alternatives Considered**:
- DynamoDB + geohashing: Requires complex sharding, poor range queries
- MongoDB with geo indexes: Less mature on AWS, limited query capabilities
- Separate Elasticsearch for geo: Added complexity, synchronization overhead

---

## ADR-002: Hybrid Search with FTS + Trigram + RRF

**Date**: 2024-11-06
**Status**: Accepted

### Context
Vietnamese restaurant names have unique characteristics:
- Diacritics (phở, bánh mì)
- Common typos (pho vs phở, bahn mi vs bánh mì)
- Mixed Vietnamese-English terms
- Partial name matching needed

### Decision
Implement hybrid search combining:
1. **Full-Text Search** (FTS) with `to_tsquery` for exact matches
2. **Trigram similarity** (`pg_trgm`) for fuzzy matching
3. **Reciprocal Rank Fusion** (RRF) to merge results

### Consequences

**Positive**:
- Handles typos and diacritics gracefully
- Works without external services (Elasticsearch, Algolia)
- Stays within PostgreSQL (simpler architecture)
- Cost-effective (no additional services)

**Negative**:
- More complex queries (100+ lines SQL)
- Requires careful index tuning
- CPU-intensive for large datasets (mitigated with caching)

**Future Consideration**:
- May migrate to pgvector with embeddings for semantic search if RRF insufficient

---

## ADR-003: Two-Table Review System (Pending + Approved)

**Date**: 2024-11-06
**Status**: Accepted

### Context
Reviews require admin moderation before appearing publicly. Need to:
- Allow users to submit reviews instantly
- Enable admin approval workflow
- Prevent premature public visibility
- Maintain audit trail

### Decision
Use separate tables:
- `review_posts`: Unmoderated submissions (pending, rejected)
- `restaurant_reviews`: Approved, public reviews

### Consequences

**Positive**:
- Clear separation of concerns
- Public queries never touch unmoderated content (security)
- Admins can query pending reviews separately
- Approved reviews immutable (trustworthy)

**Negative**:
- Duplicate schema maintenance
- Data migration on approval (INSERT + UPDATE)
- Slightly complex admin interface logic

**Alternatives Considered**:
- Single table with `status` field: Risk of accidentally exposing unmoderated content
- Approval flag only: Harder to query, index inefficiencies

---

## ADR-004: Vote Caps with Rolling 24-Hour Window

**Date**: 2024-11-06
**Status**: Accepted

### Context
Prevent vote manipulation and spam while allowing engaged users to participate.

### Decision
- 30 votes per user per 24-hour rolling window
- Tracked in `user_votes_count` table with TTL
- Enforced in `check_vote_limit()` database function

### Consequences

**Positive**:
- Prevents bot attacks
- Allows legitimate users freedom
- Server-side enforcement (secure)
- Rolling window more fair than daily reset

**Negative**:
- Requires cleanup job (EventBridge scheduled Lambda)
- Additional storage for vote tracking

---

## ADR-005: Lambda + RDS Proxy Architecture

**Date**: 2024-11-06
**Status**: Accepted

### Context
AWS Lambda is serverless and cost-effective, but PostgreSQL has connection limits. Need efficient connection management.

### Decision
- Use AWS Lambda for compute (Node.js runtime)
- Use RDS Proxy for connection pooling
- Max 100 connections in proxy pool
- Set `statement_timeout = 60s`

### Consequences

**Positive**:
- Serverless scalability
- Pay-per-use pricing (cost-effective for MVP)
- Automatic connection management
- No cold connection penalties

**Negative**:
- Cold start latency (~1s for new containers)
- RDS Proxy adds complexity ($15-20/month)
- Cannot use prepared statements across invocations

**Alternatives Considered**:
- Fargate containers: Higher baseline cost (~$30-50/month), more complex
- EC2: Need to manage servers, higher cost, not justified for MVP

---

## ADR-006: Photo Storage with S3 + CloudFront

**Date**: 2024-11-06
**Status**: Accepted

### Context
User-uploaded photos need:
- Scalable storage
- Fast global delivery
- Automatic format conversion (JPEG → WebP)
- Content moderation

### Decision
- Original uploads → S3 bucket (private)
- Lambda trigger → Rekognition moderation + Sharp resize
- Processed images → S3 public bucket
- CloudFront CDN for delivery

### Consequences

**Positive**:
- Highly scalable (petabytes if needed)
- Fast delivery with CDN (< 100ms globally)
- Automatic moderation prevents inappropriate content
- Cost-effective storage (~$0.023/GB/month)

**Negative**:
- Processing latency (2-5s per photo)
- CloudFront caching complexity
- Need to handle failed uploads

---

## ADR-007: Cognito for User Authentication

**Date**: 2024-11-06
**Status**: Accepted

**Implementation Date**: 2025-11-26

### Context
Need user authentication with:
- Email/password signup
- Social login (Google, Facebook)
- JWT token management
- Email verification

### Decision
Use AWS Cognito User Pools

### Consequences

**Positive**:
- Managed service (no auth code to write)
- Built-in social login
- JWT tokens work with API Gateway
- Compliance-ready (SOC2, HIPAA)

**Negative**:
- AWS lock-in
- Limited customization
- UI customization challenging

### Implementation Details

**Implemented Features**:
- Email/password authentication via Cognito User Pools
- Google OAuth 2.0 integration with Hosted UI
- Email verification with confirmation codes
- Session management with JWT tokens
- Password reset functionality
- AuthContext for global state management
- User session persistence with localStorage

**Components Created**:
- `LoginForm`, `SignUpForm`, `ConfirmForm`, `LoginModal`
- `AuthContext` for authentication state
- `lib/cognito.ts` for Cognito client configuration
- `AuthCallbackPage` for OAuth callback handling

**Dependencies**:
- `@aws-sdk/client-cognito-identity-provider` for direct AWS SDK integration

---

## ADR-008: Monorepo with Turborepo + Bun

**Date**: 2025-11-19
**Status**: Accepted

### Context
Need to decide project organization:
- Frontend code (React web app)
- Backend Lambda functions (future)
- Database migrations (Kysely)
- Shared types/utilities
- UI component library

Considered monorepo vs multi-repo approaches for code organization and dependency management.

### Decision
Use **Monorepo with Turborepo + Bun package manager**

Structure:
```
mapvibe/
├── apps/
│   └── web/              # Vite + React web app
├── packages/
│   ├── database/         # Kysely migrations
│   ├── ui-components/    # React component library
│   ├── types/            # Shared TypeScript types
│   └── utils/            # Shared utilities
└── turbo.json            # Turborepo config
```

### Consequences

**Positive**:
- ✅ Shared TypeScript types across packages (no version drift)
- ✅ Single source of truth for all code
- ✅ Atomic commits across frontend/backend/database
- ✅ Turborepo task caching speeds up builds
- ✅ Bun provides fast package installation (~3x faster than npm)
- ✅ Easy to refactor and move code between packages
- ✅ Single CI/CD pipeline for all packages

**Negative**:
- ⚠️ Larger repository size
- ⚠️ More complex CI/CD (must build in correct order)
- ⚠️ Learning curve for Turborepo

**Mitigations**:
- Used Turborepo pipeline orchestration for build order
- Configured GitLab CI with proper job dependencies
- Created package-specific build scripts

**Alternatives Considered**:
- Multi-repo: Rejected due to type sharing complexity
- Lerna: Rejected - Turborepo has better caching
- Nx: Rejected - too complex for current needs

---

## ADR-009: GitLab for Source Control and CI/CD

**Date**: 2025-11-19
**Status**: Accepted

### Context
Initially used GitHub, but need robust CI/CD capabilities for monorepo builds. Mentor recommended GitLab for better DevOps integration.

### Decision
Migrate from GitHub to GitLab and use GitLab CI/CD for automation.

**Repository**: `gitlab.com/4n1d/mapvibe`

**Protected Branches**:
- `prod`: Production branch, merge via MR only, no direct pushes
- `dev`: Development branch, controlled access
- `feature/*`: Feature branches for isolated development

### Consequences

**Positive**:
- ✅ Built-in CI/CD with 400 free minutes/month
- ✅ Better monorepo support (artifacts, caching)
- ✅ Integrated Docker registry
- ✅ Protected branches enforce code review
- ✅ Unlimited collaborators on Free tier
- ✅ Better AWS/Docker/K8s integration
- ✅ Security scanning built-in (SAST, dependency scanning)

**Negative**:
- ⚠️ Smaller community than GitHub
- ⚠️ Less discoverability for open source
- ⚠️ Migration effort required

**Migration Process**:
1. Used GitLab import tool to migrate repository
2. Updated local git remote from GitHub to GitLab
3. Configured protected branches
4. Setup CI/CD pipeline
5. Archived GitHub repository

**Alternatives Considered**:
- GitHub: Rejected - CI/CD less powerful, costs more for private repos with team
- Bitbucket: Rejected - less feature-rich than GitLab
- Self-hosted: Rejected - maintenance overhead

---

## ADR-010: Tailwind CSS v3 for UI Styling

**Date**: 2025-11-19
**Status**: Accepted

### Context
Need CSS framework for UI components library. Must support:
- Utility-first styling
- Component variants
- Monorepo package scanning
- Fast build times

Initially tried Tailwind v4 but encountered compatibility issues.

### Decision
Use **Tailwind CSS v3** with PostCSS approach.

**Configuration**:
- PostCSS plugin for processing
- Content scanning across monorepo packages
- Custom utility function (`cn`) for className merging

### Consequences

**Positive**:
- ✅ Stable, production-ready (v3.4.18)
- ✅ Works with PostCSS in monorepo
- ✅ Content scanning supports workspace packages
- ✅ Class merging with `tailwind-merge` prevents conflicts
- ✅ Excellent documentation and community support

**Negative**:
- ⚠️ Not latest version (v4 has issues)
- ⚠️ Requires PostCSS configuration
- ⚠️ SVG elements need inline attributes for width/height

**Issues Encountered with v4**:
- "Missing field `negated` on ScannerOptions.sources" error
- "Cannot convert undefined or null to object" with Vite plugin
- Decided to use stable v3 instead

**Implementation Details**:
- Created `packages/ui-components/src/utils/cn.ts` for smart class merging
- Configured `apps/web/tailwind.config.js` to scan ui-components package
- Used PostCSS for CSS processing instead of Vite plugin

---

## ADR-011: GitLab CI/CD Pipeline for Monorepo

**Date**: 2025-11-19
**Status**: Accepted

### Context
Monorepo requires coordinated builds across multiple packages. Need automated testing and building on every push.

### Decision
Implement GitLab CI/CD pipeline with 3 stages: install, build, test.

**Pipeline Configuration** (`.gitlab-ci.yml`):
```yaml
stages:
  - install  # Install dependencies
  - build    # Build packages in order
  - test     # Run tests (future)
```

**Jobs**:
1. `install`: Bun install with lockfile, cache node_modules
2. `build:database`: Skip (TypeScript runs directly with Bun)
3. `build:ui-components`: TypeScript compilation
4. `build:web`: Vite build for production

**Caching Strategy**:
- Cache `bun.lock` keyed dependencies
- Cache workspace `node_modules` directories
- Persist artifacts between jobs

### Consequences

**Positive**:
- ✅ Automated builds on every push
- ✅ Fast builds with caching (~3-5 min total)
- ✅ Catches build errors before merge
- ✅ Ready for automated testing
- ✅ Can extend to deployment stages

**Negative**:
- ⚠️ Limited to 400 CI/CD minutes/month on Free tier
- ⚠️ Must maintain pipeline configuration

**Build Optimizations**:
- Database package skips TypeScript build (uses Bun directly)
- UI components build before web app (dependency order)
- Workspace node_modules cached for faster installs

---

## Template for Future ADRs

```markdown
## ADR-XXX: [Title]

**Date**: YYYY-MM-DD
**Status**: Proposed | Accepted | Deprecated | Superseded

### Context
[Why we need to make this decision]

### Decision
[What we decided]

### Consequences

**Positive**:
-

**Negative**:
-

**Alternatives Considered**:
-
```

---

*This file is automatically updated by Claude CLI hooks when architectural decisions are discussed*

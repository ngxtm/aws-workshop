# MapVibe Technology Stack

> Living document tracking all technologies, frameworks, and dependencies

**Last Updated**: 2025-11-25

---

## Package Manager & Build System

### Package Manager
- **Bun**: 1.1.34+
- **Purpose**: Fast JavaScript runtime and package manager
- **Benefits**: 3x faster than npm, native TypeScript support

### Monorepo Tools
- **Turborepo**: Task orchestration and caching
- **Workspace Protocol**: Bun workspaces for package management

---

## Database Layer

### Primary Database
- **PostgreSQL**: 15.4+
- **Hosting**: AWS RDS (db.t4g.medium recommended for MVP)
- **Storage**: GP3, 100GB initial allocation
- **Extensions**:
  - `postgis` - Geospatial queries (restaurant locations within radius)
  - `pgvector` - Vector similarity search (future: semantic search)
  - `pg_trgm` - Fuzzy text matching (typo tolerance in search)
  - `uuid-ossp` - UUID generation
  - `pg_stat_statements` - Query performance monitoring

### Caching Layer
- **Redis**: AWS ElastiCache 7.0
- **Instance**: cache.t4g.medium
- **Purpose**: Session management, search result caching, rate limiting

---

## AWS Services

### Compute
- **Lambda Functions**: Node.js runtime (version TBD)
  - Restaurant search API
  - Review submission handler
  - Photo processing with Rekognition
  - Admin approval workflows
- **RDS Proxy**: Connection pooling for Lambda

### Storage
- **S3 Buckets**:
  - User uploaded photos (original)
  - Processed thumbnails (WebP format)
  - Static assets
- **CloudFront**: CDN for photo delivery

### Security & Auth
- **Cognito**: User authentication and authorization
  - User Pools for email/password authentication
  - OAuth 2.0 integration (Google social login)
  - JWT token-based session management
  - Hosted UI for social authentication flows
- **Secrets Manager**: Database credentials storage
- **IAM**: Fine-grained access control

### Networking
- **VPC**: Isolated network environment
- **API Gateway**: REST API endpoints
- **Route 53**: DNS management (if applicable)

### Monitoring
- **CloudWatch**: Logs, metrics, alarms
- **Performance Insights**: RDS query performance
- **X-Ray**: Distributed tracing (optional)

### Automation
- **EventBridge**: Scheduled jobs
  - Review voting deadline checks
  - Badge eligibility calculations
  - Analytics aggregation

---

## Backend

### Database Client
- **Kysely**: 0.27+ TypeSQL query builder
- **Purpose**: Type-safe database queries
- **Features**: Migration support, codegen for types

### Database Migrations
- **Tool**: Kysely Migrator
- **Scripts**:
  - `migrate`: Run migrations up
  - `migrate:down`: Rollback migrations
  - `codegen`: Generate TypeScript types from schema

---

## Frontend

### Web Application
- **Framework**: React 19.2.0
- **Build Tool**: Vite 7.2.2
- **TypeScript**: Latest (strict mode enabled)
- **Port**: 3000 (development)

### UI Components Library
- **Location**: `packages/ui-components`
- **Components**: Button, Card, Rating
- **Pattern**: forwardRef for component composition
- **Exports**: Named exports with TypeScript types

### Styling
- **Tailwind CSS**: 3.4.18 (latest stable v3)
- **PostCSS**: 8.5.6
- **Autoprefixer**: 10.4.22
- **Utils**: clsx + tailwind-merge for className handling
- **Configuration**: `tailwind.config.js` with monorepo content scanning
- **Note**: Using v3 instead of v4 due to stability issues (see ADR-010)

### Dependencies
```json
{
  "react": "^19.2.0",
  "react-dom": "^19.2.0",
  "@vitejs/plugin-react": "^5.1.1",
  "vite": "^7.2.2",
  "tailwindcss": "3.4.18",
  "clsx": "^2.1.1",
  "tailwind-merge": "^3.4.0",
  "react-router-dom": "^7.9.6",
  "react-responsive": "^10.0.0",
  "axios": "^1.7.9",
  "msw": "^2.6.8",
  "lucide-react": "latest",
  "@aws-sdk/client-cognito-identity-provider": "latest"
}
```

---

## Development Tools

### Database Tools
- **psql**: PostgreSQL CLI client
- **DBeaver** / **pgAdmin**: GUI database management
- **Docker**: Local PostgreSQL with PostGIS

### Infrastructure as Code
- **AWS CLI**: Resource management
- **Terraform** / **CDK** / **SAM**: Infrastructure automation (to be selected)

### CI/CD
- **GitLab CI/CD**: Automated pipeline
  - 3 stages: install, build, test
  - Bun-based builds
  - Node modules caching
  - Artifact persistence
  - 400 free minutes/month

### Version Control
- **GitLab**: Primary repository hosting
- **Repository**: `gitlab.com/4n1d/mapvibe`
- **Branches**:
  - `prod` (protected, production)
  - `dev` (protected, development)
  - `feature/*` (feature branches)

---

## Development Dependencies

### Monorepo Root
```json
{
  "turbo": "latest",
  "typescript": "^5.x"
}
```

### Database Package
```json
{
  "kysely": "^0.27.x",
  "pg": "^8.x",
  "kysely-codegen": "^0.x"
}
```

### UI Components Package
```json
{
  "react": "^19.2.0",
  "clsx": "^2.1.1",
  "tailwind-merge": "^3.4.0"
}
```

### Web App
```json
{
  "react": "^19.2.0",
  "react-dom": "^19.2.0",
  "vite": "^7.2.2",
  "tailwindcss": "^3.4.18",
  "postcss": "^8.5.6",
  "autoprefixer": "^10.4.22"
}
```

---

## Third-Party APIs

### Map Services
- **Mapbox** / **Google Maps** (to be selected)
- **Purpose**: Restaurant location display, geosearch UI

### Image Processing
- **AWS Rekognition**: Photo moderation, content detection

### Email (Optional)
- **SES**: Transactional emails (verification, notifications)

---

## Performance Targets

- **API Response Time**: < 200ms (P95)
- **Search Latency**: < 300ms (P95)
- **Database Connection Pool**: 100 max connections
- **Lambda Cold Start**: < 1s
- **Photo Upload**: < 5s (including processing)

---

## Cost Estimates (MVP)

- **RDS**: ~$70-100/month (db.t4g.medium with backups)
- **ElastiCache**: ~$40/month (cache.t4g.medium)
- **Lambda**: ~$10-30/month (1M requests)
- **S3 + CloudFront**: ~$20-50/month (10GB storage, 100GB transfer)
- **Total Estimated**: ~$150-220/month for MVP

---

## Security Compliance

- **SSL/TLS**: Enforced for all connections
- **Encryption at Rest**: RDS + S3 encryption enabled
- **IAM Roles**: Principle of least privilege
- **Secrets Rotation**: 90-day rotation policy (recommended)

---

*This file is automatically updated by Claude CLI hooks when new technologies are added*

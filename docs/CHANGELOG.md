# MapVibe Changelog

> Auto-generated changelog tracking all project modifications

## Format
Each entry includes:
- **Timestamp**: When the change occurred
- **File(s)**: Which files were modified
- **Type**: Edit, Create, or Delete
- **Description**: What changed and why

---

## Changes

### 2025-11-26

#### AWS Cognito Authentication Implementation
- **Type**: Frontend - User Authentication
- **Description**: Implemented complete AWS Cognito authentication system with social login support

  - **Authentication Components**:
    - LoginForm: Email/password login with validation
    - SignUpForm: User registration with email verification
    - ConfirmForm: Email verification code input
    - LoginModal: Modal container with tab switching between login/signup

  - **Authentication Flow**:
    - Email/password authentication via AWS Cognito
    - Social login support (Google OAuth integration)
    - Email verification with confirmation codes
    - Password reset functionality
    - Session management with JWT tokens

  - **State Management**:
    - AuthContext: Global authentication state with React Context
    - User session persistence with localStorage
    - Auto-login on page refresh if session valid
    - Sign out functionality with session cleanup

  - **Cognito Integration**:
    - Created `lib/cognito.ts` with Cognito client configuration
    - Direct AWS SDK integration (@aws-sdk/client-cognito-identity-provider)
    - OAuth callback handling with AuthCallbackPage
    - Token refresh mechanism

  - **UI Updates**:
    - Updated Header component with authentication UI
    - User profile dropdown with avatar
    - Sign in/Sign up buttons for unauthenticated users
    - Responsive authentication modal

  - **New Components**:
    - Added Input component to ui-components package
    - Form validation and error handling
    - Loading states during authentication

  - **Dependencies Added**:
    - `@aws-sdk/client-cognito-identity-provider`
    - AWS Cognito User Pool configuration in .env

  - **OAuth Configuration**:
    - Google OAuth 2.0 integration
    - Callback URL handling at `/auth/callback`
    - Hosted UI for social login

### 2025-11-25

#### PlaceDetailPage Implementation
- **Type**: Frontend - Restaurant Detail Page
- **Description**: Implemented responsive PlaceDetailPage with tab navigation

  - **ImageGalleryPreview**:
    - Desktop: 3-column layout (1 large + 2 medium + overlay)
    - Mobile: Single image view
    - react-responsive for device-based rendering
    - Hover zoom effects (scale-110)

  - **Tab System**:
    - 5 tabs: Giới thiệu, Bình luận, Nhận xét, Ảnh, Thực đơn
    - IntroductionTab has 2-column with Direction sidebar
    - Other tabs full-width

  - **RestaurantInfo Component**:
    - Redesigned with colored icon backgrounds (orange, green, blue, yellow)
    - Tags chips with rounded style
    - Improved typography and spacing
    - Clickable phone with tel: link
    - "Đang mở cửa" with pulse animation
    - Backend-calculated `isOpen` status (timezone-safe)

  - **IntroductionTab Components**:
    - ServicesList: Grid with checkmarks
    - CuisineType: Cuisine tags with description
    - SimilarPlaces: Restaurant cards grid with hover effects
    - DirectionSidebar: Sticky sidebar with Google Maps link, copy/share buttons

  - **CLAUDE.md**: Created mentor mode instructions

  - **Dependencies**: Added `react-responsive@^10.0.0`

### 2025-11-23

#### React Router & Mock Service Worker Setup
- **Type**: Frontend - Routing & API Mocking
- **Description**: Setup React Router v7 for SPA navigation and MSW for development API mocking

  - **React Router v7**:
    - Installed `react-router-dom@7.9.6`
    - Created router configuration in `apps/web/src/router/index.tsx`
    - Routes: `/`, `/place/:slug`, `/admin`
    - Updated `Layout.tsx` with `<Outlet />` for child routes
    - Updated `App.tsx` with `<RouterProvider />`
    - Fixed Vite path alias `@/` configuration

  - **Mock Service Worker (MSW)**:
    - Installed `msw@2.12.2` and `@faker-js/faker@10.1.0`
    - Created handlers in `apps/web/src/mocks/handlers.ts`
    - Mock endpoints: `/api/search`, `/api/restaurant/:slug`, `/api/restaurants`
    - Hybrid mode: Toggle between real API and mock via `VITE_USE_MOCK_SEARCH`
    - `passthrough()` for real API when mock disabled
    - Browser setup in `apps/web/src/mocks/browser.ts`
    - MSW initialization in `main.tsx`

  - **Environment Variables**:
    - Created `.env` with `VITE_API_URL` and `VITE_USE_MOCK_SEARCH`
    - Created `.env.example` template
    - Added Vite env type definitions in `vite-env.d.ts`
    - Fixed `import.meta.env` TypeScript errors

  - **URL Structure**:
    - Changed from `/restaurant/:id` to `/place/:slug` (SEO-friendly)
    - Slug-based routing for better URLs

---

#### Axios HTTP Client Migration
- **Type**: Frontend - HTTP Client
- **Description**: Migrated from Fetch API to Axios with advanced features

  - **Axios Setup**:
    - Created `apps/web/src/lib/axios.ts` with centralized configuration
    - Base URL configuration (ngrok API endpoint)
    - Global headers setup (Content-Type, ngrok-skip-browser-warning)
    - Request timeout: 15 seconds

  - **Request/Response Interceptors**:
    - Request logging with emoji indicators (🚀 for requests)
    - Response logging (✅ for success, ❌ for errors)
    - Automatic error handling by status code (401, 403, 404, 500)
    - Network error detection

  - **Advanced Features**:
    - **Cancel Token**: AbortController for cancelling requests
      - Hủy request cũ khi user gửi message mới nhanh
      - Tránh race condition (response cũ hiện lên sau response mới)
    - **Retry Logic**: Automatic retry (3 attempts, 1s delay)
      - `retryRequest` helper function
      - Graceful fallback to mock data on final failure
    - **Signal-based Abort**: Uses native Fetch AbortController API

  - **useSearchChat Integration**:
    - Updated `sendMessage` callback with full Axios features
    - Cancel token management with `useRef`
    - Retry logic with exponential backoff
    - Error logging with detailed messages
    - Updated `clearMessages` to cancel pending requests

  - **Benefits over Fetch**:
    - Cleaner syntax (no manual JSON parsing)
    - Auto status code checking
    - Built-in request/response interceptors
    - Better error handling
    - Centralized configuration

---

### 2025-11-22

#### Real API Integration & Project Management
- **Type**: Integration & DevOps
- **Description**: Integrated real search API and setup project management

  - **Search API Integration**:
    - Replaced mock responses with real API calls
    - API endpoint: ngrok tunnel to team AI's backend
    - Session-based context management (`session_id`)
    - `is_new_topic` flag for conversation reset
    - Fallback to mock data on API error

  - **CORS Configuration**:
    - Documented CORS setup for FastAPI
    - Added manual OPTIONS handler for preflight requests
    - Troubleshooting guide for common CORS issues

  - **GitLab → GitHub Mirror**:
    - Setup repository mirroring from GitLab to GitHub
    - Automatic sync every 5 minutes
    - Using GitHub Personal Access Token

  - **Jira Project Management**:
    - Created 4 Epics: Search & Restaurant, User & Reviews, Admin Dashboard, Launch Prep
    - 3 Sprints planned (21/11 - 11/12)
    - Task-based workflow for 5-person team
    - Deadline: 11/12 (launch: 24/12)

---

### 2025-11-21

#### Feature-based Architecture & Conversational AI Search
- **Type**: Frontend Architecture
- **Description**: Implemented feature-based code organization and ChatGPT-style conversational search
  - **Feature-based Structure**:
    - `features/search/` - Search feature with components, hooks, and index
    - `features/review/` - Review feature with FeaturedReviews
    - `pages/` - Page-level components (HomePage)
    - Clear separation of concerns by business domain

  - **Conversational Search (RAG-ready)**:
    - `HeroSearch.tsx`: Auto-expanding textarea with multiline detection
    - `ChatMessage.tsx`: Message bubbles with restaurant cards
    - `SearchConversation.tsx`: Conversation container with auto-scroll
    - `useSearchChat.ts`: State management with mock RAG responses
    - One-way multiline transition to prevent flickering

  - **FeaturedReviews Section**:
    - Trending reviews grid (6 items, 3-column on desktop)
    - Star ratings and user avatars
    - Hidden when conversation is active

  - **Parallax Scroll Effects**:
    - Hero section shrinks from 100vh to 50vh when searching
    - Content moves at different speeds based on scroll
    - Smooth opacity transitions

  - **Auto-scroll Bug Fixes**:
    - Fixed conflict between HomePage and SearchConversation scroll
    - HomePage scrolls on first message only
    - SearchConversation scrolls on subsequent messages
    - Changed scroll target to user's question (not bottom of page)

  - **UI/UX Improvements**:
    - Loading indicator with spinning icon
    - Restaurant cards with image placeholder, price, hours, match reason
    - Sticky input at bottom of conversation
    - Timestamp display in Vietnamese format

  - **Accumulated Search Context**:
    - `buildSearchContext` joins all user queries for AI search
    - Simplified approach instead of full RAG
    - Format: `"query1 | query2 | query3"`

  - **Reset Conversation Feature**:
    - Added reset button (RotateCcw icon) in input area
    - `clearMessages` function to reset conversation
    - Returns user to FeaturedReviews view

---

### 2025-11-20

#### App Layout & Homepage Development
- **Type**: Frontend
- **Description**: Created responsive app layout and homepage hero section
  - **Layout Components**:
    - `Header.tsx`: Sticky header with logo, navigation, auth buttons, mobile menu
    - `Footer.tsx`: 4-column footer with brand info, quick links, social icons
    - `Layout.tsx`: Main layout wrapper with flexbox structure
    - Navigation with underline hover animation effect

  - **Homepage Hero Section**:
    - Full-height hero with background image
    - Dark overlay for better text readability
    - Heading text với drop-shadow

  - **Search Box (ChatGPT-style)**:
    - Auto-expanding textarea với smooth height transition
    - SendButton với ArrowUp icon (lucide-react)
    - Vertically centered button positioning
    - Custom scrollbar hiding (Chrome, Firefox, IE/Edge)
    - Enter to submit, Shift+Enter for new line
    - Focus states với ring effect

  - **UI Components Added**:
    - `SendButton`: Circular send button with ArrowUp icon
    - Integrated lucide-react icons (Search, ChevronsDown, ArrowUp)

  - **Scroll Indicator**:
    - Bounce animation at bottom of hero
    - ChevronsDownIcon với drop-shadow

  - **Tailwind Configuration**:
    - Updated brand colors based on UI design mockups
    - Primary: #E85A4F (đỏ cam)
    - Secondary: #22c55e (xanh lá cho giờ mở cửa)
    - Accent: #f97316 (cam cho giá)

#### Code Quality Setup
- **Type**: Infrastructure
- **Description**: Configured Prettier for code formatting
  - Created `.prettierrc` with project settings
  - Configured `singleAttributePerLine: true` for JSX
  - Set `printWidth: 80` for earlier line wrapping
  - Added `prettier-plugin-tailwindcss` for class sorting (optional)

---

### 2025-11-19

#### GitLab Migration & CI/CD Setup
- **Type**: Infrastructure
- **Description**: Migrated repository from GitHub to GitLab with complete CI/CD setup
  - **Repository Migration**:
    - Imported full repository from GitHub to GitLab (gitlab.com/4n1d/mapvibe)
    - Migrated all branches: prod, dev, feature/add-homepage
    - Updated local git remote from GitHub to GitLab
    - Archived GitHub repository

  - **Protected Branches Configuration**:
    - Protected `prod` branch: No direct pushes, Merge Request required
    - Protected `dev` branch: Controlled access with proper permissions
    - Enforced code review workflow

  - **GitLab CI/CD Pipeline**:
    - Created `.gitlab-ci.yml` with 3 stages: install, build, test
    - Install stage: Bun dependency installation with caching
    - Build stage: Automated builds for database, ui-components, web packages
    - Configured workspace node_modules caching for faster builds
    - Pipeline successfully passing on all jobs

  - **Build Configuration**:
    - Added TypeScript build scripts to packages/database and packages/ui-components
    - Created package-specific tsconfig.json files
    - Optimized database package to skip build (uses TypeScript directly with Bun)
    - Fixed Vite build issues in web app

#### Monorepo Setup
- **Type**: Project Structure
- **Description**: Configured Turborepo monorepo with Bun package manager
  - **Packages**:
    - `packages/database` - Kysely migrations and schema
    - `packages/ui-components` - React component library with Tailwind CSS
    - `packages/types` - Shared TypeScript types
    - `apps/web` - Vite + React web application

  - **Build System**:
    - Turborepo for task orchestration
    - Bun for fast package management
    - TypeScript compilation for all packages

#### UI Components Library
- **Type**: Frontend
- **Description**: Created reusable React component library
  - **Components**:
    - Button: 4 variants (primary, secondary, danger, ghost), 3 sizes
    - Card: Modular card with Header, Title, Content sub-components
    - Rating: Interactive star rating with read-only mode

  - **Styling**:
    - Tailwind CSS v3 with PostCSS
    - Smart className merging with clsx + tailwind-merge
    - Monorepo content scanning for styles

  - **Fixed Issues**:
    - SVG rendering: Changed from Tailwind classes to inline attributes
    - TypeScript JSX configuration
    - Tailwind v4 compatibility issues (downgraded to v3)

#### Database Configuration
- **Type**: Database
- **Description**: Enhanced database package with rollback support
  - Added `migrateDown()` function for migration rollback
  - Command-line argument parsing with `process.argv`
  - `migrate:down` script for easy rollback execution
  - Fixed codegen script port (5432 → 5433)

#### Documentation System
- **Type**: Documentation
- **Description**: Created slash command system for automated documentation updates
  - Created `.claude/commands/docs.md` slash command
  - Command auto-updates: CHANGELOG, ARCHITECTURE_DECISIONS, TECH_STACK, context.md
  - Integrated with Claude Code CLI workflow

#### Tailwind CSS v3 Configuration
- **Type**: Frontend
- **Description**: Properly configured Tailwind CSS v3 with stable dependencies
  - **Dependencies Updated**:
    - `tailwindcss@3.4.18` (latest stable v3, avoiding v4 bugs)
    - `autoprefixer@10.4.22` (latest)
    - `postcss@8.5.6` (latest)

  - **Configuration Files**:
    - Created `apps/web/tailwind.config.js` with content sources
    - Configured content scanning for monorepo packages
    - Added MapVibe brand colors (primary, secondary palettes)
    - PostCSS integration via `postcss.config.mjs`

  - **Fixed Issues**:
    - Resolved "content option missing or empty" warning
    - Configured proper content paths: `./src/**/*.{js,ts,jsx,tsx}`, `../../packages/ui-components/src/**/*`
    - Build verified: 10.00 kB CSS output, gzip: 2.64 kB

#### .gitignore Optimization
- **Type**: Infrastructure
- **Description**: Fixed overly broad .gitignore patterns
  - **Before**: `*.js` ignored ALL .js files globally (broke config files)
  - **After**: Only ignore compiled outputs in `dist/`, `build/` directories
  - **Protected Files**: Added negation patterns for important configs
    - `!*.config.js`, `!*.config.mjs`
    - `!tailwind.config.js`, `!postcss.config.js`
    - `!vite.config.js`, `!jest.config.js`
  - Verified `tailwind.config.js` now tracked by git

---

### 2025-11-13

#### Initial Setup
- **Type**: Project Initialization
- **Files**: Multiple database design files
- **Description**: MapVibe restaurant review platform database design completed
  - PostgreSQL 15+ with PostGIS, pgvector, pg_trgm extensions
  - 11 core tables for restaurants, reviews, users, ratings
  - RDS architecture with Lambda integration
  - Full-text search and geospatial capabilities

---

*This file is automatically updated by Claude CLI hooks*

# 12-Week .NET Backend + Angular Growth Plan

Nov 6, 2026

## How to use this plan

Over 12 weeks you build one real system, Field_Ops (a Field Service Management app), and every week adds one backend skill and one Angular skill to it. Budget about 10 to 12 hours a week next to your job.

**Weekly rhythm**

| Day | Focus | Time |
| --- | --- | --- |
| Sun to Wed | Backend topic of the week: learn, then build it in Field_Ops | 1 to 1.5 h/day |
| Thu | Code review with Claude, fix what it finds | 1 h |
| Sat | Angular topic of the week, connect it to the API | 3 h |
| Sun (evening) | Quiz + write a short "what I learned" note in the README | 30 min |

**Rules**

1. You write the code. Claude explains, hints, reviews and quizzes. Use Claude Code only for boilerplate you already understand.
2. Try for 20 to 30 minutes before asking for help.
3. Every week ends with a merged pull request, passing tests, and a short note in the README.
4. If a week runs over, finish the backend part first; the Angular part can spill into the next week.
5. Keep one Claude Project called ".NET Mentor" with this plan and your repo's README attached, so Claude always knows where you are.

**Mentor instructions to paste into the Project**

> I'm a Level 1 backend .NET developer following a 12-week plan to build Field_Ops (.NET + Angular). Act as my senior mentor. Don't write full solutions unless I ask. Give hints first, ask me questions, and make me explain my reasoning. Review my code like a strict senior in a real PR. Always explain why.

## The project: Field_Ops

Field_Ops is a Field Service Management system: a company receives service requests from customers, a dispatcher creates work orders and assigns them to technicians, technicians complete the jobs, and the company invoices the customer.

**Users**

| Role | What they do |
| --- | --- |
| Admin | Manages users, roles, service types and prices |
| Dispatcher | Creates work orders, assigns technicians, watches the live board |
| Technician | Sees assigned jobs, updates status, logs parts used and notes |
| Customer | Submits service requests and tracks their status |

**Core modules**

| Module | Main entities | Built in |
| --- | --- | --- |
| Customers and sites | Customer, Site (address, contact) | Week 1 |
| Work orders | WorkOrder, status workflow, priority | Weeks 2 to 3 |
| Identity | User, Role, JWT, refresh tokens | Week 4 |
| Scheduling and dispatch | Assignment, technician availability | Week 6 |
| Parts and inventory | Part, StockMovement, PartUsage | Week 7 |
| Invoicing | Invoice, InvoiceLine | Week 8 |
| Notifications | Email / real-time alerts via SignalR | Week 9 |
| Reports dashboard | Jobs per technician, revenue, SLA breaches | Week 11 |

**Tech stack**

- Backend: ASP.NET Core Web API on .NET 10, Clean Architecture, CQRS with MediatR, FluentValidation, EF Core + PostgreSQL
- Frontend: Angular (latest version) with standalone components, signals, reactive forms, Angular Material
- Testing: xUnit, NSubstitute or Moq, Testcontainers for PostgreSQL, Playwright for a few end-to-end tests
- Infrastructure: Docker Compose, GitHub Actions, Serilog, Redis (cache), Hangfire (background jobs)

**Repository layout**

```
Field_Ops/
  src/
    Field_Ops.Domain/
    Field_Ops.Application/
    Field_Ops.Infrastructure/
    Field_Ops.Api/
  tests/
    Field_Ops.UnitTests/
    Field_Ops.IntegrationTests/
  web/                # Angular app
  docs/
    PLAN.md           # this file
  docker-compose.yml
  CLAUDE.md
  README.md           # weekly learning notes live here
```

## Phase 1 (Weeks 1 to 4): Solid foundations

By the end of week 4, Field_Ops has customers, work orders, tests and login, and you can build a basic Angular screen on your own.

### Week 1: Project setup + Customers and Sites

**Backend:** solution structure in Clean Architecture, PostgreSQL in Docker, EF Core DbContext and migrations, first commands and queries with MediatR, CORS.

**Angular:** install Node and Angular CLI, TypeScript basics, standalone components, routing, HttpClient. Build a Customers list page that calls your API.

- [ ] Solution with 4 projects compiles; Postgres runs in Docker Compose
- [ ] CRUD endpoints for Customer and Site
- [ ] Angular app shows the customer list and a customer details page

**Ask Claude:** "Explain the dependency rule in Clean Architecture, then quiz me on where each class in my project should live."

### Week 2: Work orders, validation and error handling

**Backend:** WorkOrder entity with status workflow (New, Assigned, InProgress, Completed, Cancelled), FluentValidation as a MediatR pipeline behavior, global exception handling with ProblemDetails, the Result pattern.

**Angular:** reactive forms, form validation and error messages, showing API validation errors in the form.

- [ ] Work order create, update, change-status endpoints
- [ ] Invalid status changes are rejected by the domain, not the controller
- [ ] Angular "New work order" form with validation

**Ask Claude:** "Review my WorkOrder entity. Is the business logic in the right place, or is my domain anemic?"

### Week 3: Testing

**Backend:** unit tests for domain rules and handlers (xUnit, mocking), integration tests with WebApplicationFactory and Testcontainers for PostgreSQL, the Arrange-Act-Assert pattern.

**Angular:** services and dependency injection, RxJS basics (Observable, pipe, map, switchMap, catchError), a first component test.

- [ ] At least 15 unit tests covering the status workflow
- [ ] 5 integration tests hitting a real Postgres container
- [ ] Angular data access moved into services

**Ask Claude:** "Here are my tests. What important cases am I missing? Which tests are testing implementation instead of behavior?"

### Week 4: Authentication and authorization

**Backend:** ASP.NET Core Identity, JWT access tokens, refresh tokens, role-based and policy-based authorization (Admin, Dispatcher, Technician, Customer), password hashing basics.

**Angular:** login page, auth service, HTTP interceptor that attaches the token, route guards per role, signals for the current user.

- [ ] Login, refresh and logout endpoints
- [ ] Every endpoint protected by the right role or policy
- [ ] Angular hides pages the user's role cannot access

**Ask Claude:** "Attack my auth design. What could a hacker do with my tokens, and how do I prevent it?"

## Phase 2 (Weeks 5 to 8): Production-grade backend

By the end of week 8, Field_Ops handles real data volumes, concurrent users and money correctly, and the Angular app has a proper structure.

### Week 5: EF Core performance and SQL

**Backend:** seed 100,000 work orders, then find and fix slow queries. Learn N+1 problems, AsNoTracking, projections with Select, pagination, filtering and sorting, indexes, and reading query plans with EXPLAIN ANALYZE.

**Angular:** Angular Material table with server-side pagination, sorting and filtering.

- [ ] Work order list endpoint responds in under 200 ms with 100k rows
- [ ] You can explain one query plan before and after adding an index
- [ ] Angular work order list with paging and filters

**Ask Claude:** "Here is the SQL EF Core generated and the query plan. Guide me to the bottleneck with questions."

### Week 6: Scheduling and dispatch

**Backend:** assign technicians to work orders, technician availability, preventing double booking, optimistic concurrency (PostgreSQL xmin as row version), handling DbUpdateConcurrencyException.

**Angular:** dispatcher board with Angular CDK drag and drop, signals and computed() for board state.

- [ ] Two dispatchers assigning the same technician at the same time cannot both succeed
- [ ] A test proves the concurrency rule
- [ ] Dispatcher can drag a job onto a technician

**Ask Claude:** "Explain optimistic vs pessimistic concurrency with a Field_Ops example, then let me choose and defend one."

### Week 7: Parts, inventory and async in depth

**Backend:** stock movements inside transactions, isolation levels, aggregate boundaries, domain events. Deep dive into async/await: what really happens, CancellationToken everywhere, why .Result and .Wait() cause trouble.

**Angular:** lazy-loaded feature routes, shared components, smart vs presentational components, input() and output().

- [ ] Using a part on a job reduces stock in the same transaction
- [ ] Stock can never go negative, even under parallel requests
- [ ] Angular app split into lazy-loaded features

**Ask Claude:** "Quiz me with 10 tricky async/await questions, one at a time."

### Week 8: Invoicing and the customer portal

**Backend:** generate invoices from completed work orders, handle money with decimal and rounding rules, PDF generation with QuestPDF, idempotency keys so an invoice is never created twice.

**Angular:** customer portal (submit request, track status, download invoice), state management with signals or NgRx SignalStore.

- [ ] Invoice PDF downloads correctly
- [ ] Sending the same request twice creates one invoice
- [ ] Customer can submit and track a request end to end

**Ask Claude:** "Review my invoicing code like it handles real money. What would break in production?"

## Phase 3 (Weeks 9 to 12): Reliability, operations and shipping

By the end of week 12, Field_Ops is deployed, monitored and tested end to end, and you can explain every design decision in an interview.

### Week 9: Background jobs, notifications and real time

**Backend:** Hangfire for background jobs (SLA breach checks, reminder emails), the Outbox pattern so events are never lost, SignalR hub for live updates.

**Angular:** SignalR client, live updates on the dispatcher board, toast notifications.

- [ ] When a technician changes a job status, the dispatcher board updates without refresh
- [ ] A recurring job flags work orders past their SLA
- [ ] Events go through an outbox table

**Ask Claude:** "What happens to my notifications if the app crashes right after saving to the database? Walk me through it."

### Week 10: Observability and resilience

**Backend:** structured logging with Serilog, correlation IDs, OpenTelemetry tracing, health checks, Polly retries and circuit breakers for external calls, rate limiting.

**Angular:** global error handler, error interceptor, loading and empty states on every page.

- [ ] Every request has a correlation ID you can trace through the logs
- [ ] /health endpoint checks Postgres and Redis
- [ ] No Angular page shows a blank screen on error

**Ask Claude:** "Here is a production-style log. Act as on-call and help me find the root cause with questions only."

### Week 11: Caching and reports

**Backend:** Redis distributed cache, output caching, cache invalidation, reporting queries with raw SQL or Dapper, CTEs and window functions.

**Angular:** dashboard with charts (jobs per technician, revenue, SLA breaches), performance with OnPush change detection, @for with track, and @defer.

- [ ] Dashboard endpoint uses cache and invalidates correctly
- [ ] One report written with a window function
- [ ] Angular dashboard loads quickly with deferred charts

**Ask Claude:** "Give me 5 SQL challenges on my Field_Ops schema, from easy to hard."

### Week 12: Ship it

**Backend:** multi-stage Dockerfile, Docker Compose for the full stack, GitHub Actions pipeline (build, test, lint), deployment (Render works well since you already know it), OWASP security review.

**Angular:** production build, environment configs, 3 Playwright end-to-end tests for the main flows.

- [ ] Every push runs build and all tests
- [ ] App is live on a public URL
- [ ] README has architecture overview, setup steps and screenshots

**Ask Claude:** "Run a 45-minute mock system design interview: design Field_Ops for 10,000 technicians. Grade me at the end."

## Progress tracker

Update the status every Sunday and add one line on what was hardest; Claude can use this to adjust the next week.

| Week | Backend topic | Angular topic | Status | Hardest part |
| --- | --- | --- | --- | --- |
| 1 | Setup, Clean Architecture, EF Core | CLI, components, routing, HttpClient | Not started |  |
| 2 | Validation, error handling, domain logic | Reactive forms | Not started |  |
| 3 | Unit and integration testing | Services, DI, RxJS | Not started |  |
| 4 | Identity, JWT, authorization | Interceptors, guards, signals | Not started |  |
| 5 | EF Core performance, SQL, indexes | Server-side table paging | Not started |  |
| 6 | Concurrency, scheduling | CDK drag and drop, computed signals | Not started |  |
| 7 | Transactions, domain events, async | Lazy loading, component design | Not started |  |
| 8 | Money, PDFs, idempotency | State management, customer portal | Not started |  |
| 9 | Hangfire, Outbox, SignalR | Real-time UI, toasts | Not started |  |
| 10 | Logging, tracing, Polly, health checks | Error handling, loading states | Not started |  |
| 11 | Redis, reports, advanced SQL | Dashboard charts, performance | Not started |  |
| 12 | Docker, CI/CD, deployment, security | Production build, Playwright E2E | Not started |  |

**Start of each week, paste this to Claude:**

> I'm starting week [N] of my Field_Ops plan. Topic: [backend topic] and [Angular topic]. Last week the hardest part was [X]. Teach me the key concepts in small steps, then give me the first task to build myself.

**End of each week:**

> Here is my PR for week [N]. Review it like a strict senior engineer, then quiz me with 5 questions on this week's topics.

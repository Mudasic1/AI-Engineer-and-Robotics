Bilkul. Is project ko tutorial ki tarah nahi, production-grade engineering project ki tarah build karte hain.

🚀 Project Roadmap

AI Sales Automation Platform

Goal: Ek SaaS platform jo lead ko receive kare, enrich kare, AI se qualify/score kare, personalized outreach banaye, CRM mein sync kare aur asynchronous workflows ke through automate kare.

┌──────────────────┐
                    │   Web Dashboard  │
                    └────────┬─────────┘
                             │
                         HTTPS/API
                             │
                    ┌────────▼─────────┐
                    │   API Backend    │
                    │    FastAPI       │
                    └────────┬─────────┘
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
       PostgreSQL          Redis             S3
          │                  │
          └──────────────────┼──────────────────┐
                             │                  │
                           SQS              AI Worker
                             │                  │
                             └────────┬─────────┘
                                      │
                              AI Lead Analysis
                                      │
                    ┌─────────────────┼──────────────┐
                    │                 │              │
                 HubSpot            Email        Analytics


---

PHASE 0 — Engineering Stack

Don't start coding randomly.

Backend

Python

FastAPI

Pydantic

SQLAlchemy

Alembic

PostgreSQL

Redis

Celery/RQ or AWS SQS-based workers


AI

LLM API

Structured outputs

Prompt versioning

Evaluation

Guardrails

Retry/fallback


Frontend

Next.js

TypeScript

Tailwind CSS


Auth

OAuth 2.0 / OpenID Connect

Google Login

Secure sessions


Cloud

AWS

ECS/Fargate

RDS

S3

SQS

ECR

CloudWatch

Secrets Manager

IAM

Route 53

CloudFront


DevOps

Docker

GitHub

GitHub Actions

Terraform

pytest

Ruff

mypy

security scanning



---

PHASE 1 — Create the Project

Step 1 — Create GitHub repository

Create:

ai-sales-automation

Don't immediately start with AWS.

First build locally.


---

Step 2 — Create project structure

ai-sales-automation/
│
├── backend/
│   ├── app/
│   │   ├── api/
│   │   ├── core/
│   │   ├── models/
│   │   ├── schemas/
│   │   ├── services/
│   │   ├── repositories/
│   │   ├── workers/
│   │   └── main.py
│   │
│   ├── tests/
│   ├── alembic/
│   ├── pyproject.toml
│   └── Dockerfile
│
├── frontend/
│
├── infrastructure/
│   └── terraform/
│
├── .github/
│   └── workflows/
│
├── docs/
│
├── docker-compose.yml
├── .env.example
├── .gitignore
└── README.md


---

PHASE 2 — Local Development Environment

Step 3 — Install prerequisites

Install:

Git
Python 3.12+
Node.js LTS
Docker Desktop
VS Code
AWS CLI
Terraform

Then verify:

git --version
python --version
node --version
docker --version
aws --version
terraform --version


---

PHASE 3 — Backend

Step 4 — Create Python environment

Inside backend:

python -m venv .venv

Activate it.

Windows:

.venv\Scripts\activate

Linux/macOS:

source .venv/bin/activate


---

Step 5 — Install dependencies

Core:

pip install fastapi uvicorn

Database:

pip install sqlalchemy psycopg[binary] alembic

Validation/config:

pip install pydantic pydantic-settings

Auth/security:

pip install authlib python-jose passlib[bcrypt]

HTTP:

pip install httpx

AI:

pip install openai

Testing:

pip install pytest pytest-asyncio

Development:

pip install ruff mypy

Then freeze/manage dependencies properly in pyproject.toml.


---

PHASE 4 — Database

Step 6 — Run PostgreSQL with Docker

Your docker-compose.yml should eventually contain:

PostgreSQL
Redis
Local services

Start:

docker compose up -d

Check:

docker ps


---

Step 7 — Design database

Start with:

users

id
email
name
provider
provider_user_id
created_at
updated_at

organizations

id
name
created_at

leads

id
organization_id
name
email
company
website
job_title
source
status
created_at
updated_at

lead_scores

id
lead_id
score
classification
reasoning
model
prompt_version
created_at

outreach

id
lead_id
channel
subject
body
status
sent_at
created_at

oauth_connections

id
organization_id
provider
access_token
refresh_token
expires_at
scopes
created_at

audit_logs

id
organization_id
user_id
action
resource
metadata
created_at


---

PHASE 5 — Database Migrations

Step 8 — Configure Alembic

Create:

alembic init alembic

Then create migrations.

Your workflow:

Change model
     ↓
Create migration
     ↓
Review migration
     ↓
Apply migration

Commands:

alembic revision --autogenerate -m "create leads"

Then:

alembic upgrade head

Never manually modify production databases.


---

PHASE 6 — API Architecture

Step 9 — Create FastAPI application

Start with:

GET /health

Response:

{
  "status": "ok"
}

Then create API versioning:

/api/v1/

Final architecture:

/api/v1/auth
/api/v1/leads
/api/v1/outreach
/api/v1/integrations
/api/v1/analytics


---

PHASE 7 — Authentication

Step 10 — Google OAuth 2.0

Create Google OAuth application.

Flow:

Frontend
   │
   │ Login
   ▼
Backend
   │
   ▼
Google Authorization
   │
   ▼
Authorization Code
   │
   ▼
Backend Callback
   │
   ▼
Exchange Code
   │
   ▼
User Identity
   │
   ▼
Create/Login User

Implement:

Authorization URL
State
Callback
Token exchange
User information
Session
Logout


---

🔐 OAuth Security

This is where you move from beginner to professional.

Implement:

State parameter

Prevents CSRF-style OAuth attacks.

PKCE

Use where applicable.

Secure cookies

HttpOnly
Secure
SameSite

Token protection

Never:

print(access_token)
commit(access_token)
return(access_token)

Store secrets securely.


---

PHASE 8 — Lead Management

Step 11 — CRUD

Build:

POST /api/v1/leads
GET /api/v1/leads
GET /api/v1/leads/{id}
PATCH /api/v1/leads/{id}
DELETE /api/v1/leads/{id}

Add:

Pagination
Filtering
Sorting
Validation
Authorization

Example:

GET /leads?status=hot&page=1&limit=20


---

PHASE 9 — AI ENGINE

Now the interesting part.

Step 12 — Build AI service abstraction

Don't put AI calls directly inside routes.

Bad:

API route → OpenAI

Better:

API
 ↓
LeadService
 ↓
AIService
 ↓
LLM Provider

Create:

services/
    ai/
        base.py
        provider.py
        prompts.py
        schemas.py

This lets you switch models later.


---

Step 13 — Lead scoring

Input:

{
  "name": "John Smith",
  "job_title": "VP Sales",
  "company": "ABC Inc",
  "website": "abc.com"
}

AI output:

{
  "score": 87,
  "classification": "very_hot",
  "reasons": [
    "Decision maker",
    "Strong ICP match"
  ],
  "recommended_action": "personalized_email"
}

Use structured output.

Don't depend on:

"John looks like a good lead..."


---

PHASE 10 — AI-Assisted Development

This project specifically trains you in AI-assisted engineering.

For every feature:

Requirement
 ↓
Ask AI to propose architecture
 ↓
AI generates implementation
 ↓
You inspect code
 ↓
Run tests
 ↓
Security review
 ↓
Refactor
 ↓
Commit

Your AI prompts should ask for:

Architecture
Implementation
Tests
Edge cases
Security risks
Performance considerations

Not:

> "Build my entire application."




---

PHASE 11 — Prompt Engineering

Create prompt versions:

prompts/
├── lead_scoring/
│   ├── v1.txt
│   ├── v2.txt
│
└── personalization/
    ├── v1.txt

Store:

prompt_version
model
temperature/settings
input
output

This gives you reproducibility.


---

PHASE 12 — Personalized Outreach

Step 13

Create:

POST /api/v1/leads/{id}/personalize

AI receives:

Lead
Company
Job title
Company information
Lead score

Generates:

Subject
Email body
CTA

Example:

Hi John,

I noticed ABC Inc is expanding its sales organization...

Would it make sense to discuss how...


---

PHASE 13 — Sales Integration

Step 14 — HubSpot OAuth

Now implement a second OAuth integration.

Architecture:

Your App
   ↓
Connect HubSpot
   ↓
OAuth 2.0
   ↓
Access Token
   ↓
HubSpot API

Create an abstraction:

CRMProvider
    │
    ├── HubSpotProvider
    └── FutureCRMProvider

This is important.

Don't hard-code your entire system around HubSpot.


---

PHASE 14 — Email Integration

Implement an email provider abstraction:

EmailProvider
    │
    └── SMTP/API Provider

Then:

Lead
 ↓
AI personalization
 ↓
Email provider
 ↓
Send
 ↓
Record result

Handle:

success
failure
retry
rate limits
bounce
duplicate sending


---

PHASE 15 — Event-Driven Architecture

Now upgrade the application.

Instead of:

POST /lead
 ↓
AI
 ↓
Email
 ↓
CRM

use events.

Lead Created
      ↓
     SQS
      ↓
Enrichment Worker
      ↓
Lead Enriched
      ↓
     SQS
      ↓
AI Worker
      ↓
Lead Scored
      ↓
     SQS
      ↓
Outreach Worker

This is a major step toward production architecture.


---

PHASE 16 — AWS SQS

Create queues:

lead-enrichment
lead-scoring
outreach
dead-letter

Implement:

Producer
Consumer
Retry
Visibility timeout
Dead-letter queue
Idempotency


---

PHASE 17 — Background Workers

Your application becomes:

FastAPI
   │
   └── publishes job
             ↓
            SQS
             ↓
          Worker
             ↓
       AI/API processing

The API should not wait 20–30 seconds for AI processing.

Return:

{
  "job_id": "123",
  "status": "queued"
}


---

PHASE 18 — Docker

Create backend Dockerfile.

Build:

docker build -t ai-sales-api .

Run:

docker run ...

Then local environment:

Frontend
Backend
PostgreSQL
Redis
Worker

all through Docker Compose.


---

PHASE 19 — Testing

Target:

Unit tests
Integration tests
API tests
OAuth tests
AI tests
Worker tests

Example:

tests/
├── unit/
├── integration/
├── api/
├── auth/
├── ai/
└── workers/

Test things like:

Invalid lead
Duplicate lead
Unauthorized user
OAuth failure
Expired token
AI malformed output
SQS failure
Email failure
CRM failure


---

PHASE 20 — AI Evaluation

This is very important for an AI Engineer.

Create a dataset:

100 leads

For each:

lead
expected classification
expected score range

Then test your AI system.

Metrics:

Score accuracy
Classification accuracy
JSON validity
Hallucination rate
Latency
Cost per lead

Now you're doing AI engineering, not simply API calling.


---

PHASE 21 — Observability

Implement structured logs:

{
  "request_id": "...",
  "user_id": "...",
  "action": "lead_score",
  "lead_id": "...",
  "latency_ms": 1240
}

Track:

API latency
AI latency
AI failures
SQS failures
Email failures
CRM failures

AWS:

CloudWatch


---

PHASE 22 — Security

Before AWS production:

Application

Input validation
Authentication
Authorization
Rate limiting
CORS
CSRF protection where applicable
SQL injection prevention
Secure headers

AWS

IAM least privilege
Secrets Manager
Private RDS
Security Groups
Encryption
HTTPS
CloudWatch auditing

OAuth

State
PKCE where applicable
Redirect URI validation
Token expiry
Token revocation
Secure storage


---

PHASE 23 — AWS Infrastructure

Now create:

VPC
├── Public subnet
│
└── Private subnets
    ├── ECS
    └── RDS

Services:

Route 53
CloudFront
ALB
ECS/Fargate
ECR
RDS PostgreSQL
S3
SQS
CloudWatch
Secrets Manager
IAM


---

PHASE 24 — Terraform

Structure:

terraform/
├── modules/
│   ├── networking/
│   ├── ecs/
│   ├── database/
│   ├── queue/
│   └── monitoring/
│
├── environments/
│   ├── dev/
│   └── prod/
│
├── main.tf
├── variables.tf
├── outputs.tf
└── providers.tf

Workflow:

terraform fmt
terraform validate
terraform plan
terraform apply


---

PHASE 25 — CI/CD

GitHub Actions:

Git Push
   ↓
Lint
   ↓
Type Check
   ↓
Unit Tests
   ↓
Integration Tests
   ↓
Security Scan
   ↓
Docker Build
   ↓
Push → ECR
   ↓
Deploy → ECS

Separate:

Development
Staging
Production


---

PHASE 26 — Frontend

Build dashboard:

Login
 │
 └── Dashboard
       │
       ├── Leads
       ├── Lead Details
       ├── AI Score
       ├── Outreach
       ├── CRM
       ├── Integrations
       └── Analytics

Lead page:

John Smith
VP Sales
ABC Inc

AI Score
87 / 100

Classification
VERY HOT

Why?
✓ Decision maker
✓ ICP match
✓ Buying signal

Recommended action
Personalized email


---

PHASE 27 — Analytics

Dashboard:

Total Leads
     1,248

Hot Leads
       183

Emails Sent
       734

Response Rate
      18.4%

Conversion
       7.8%

Charts:

Leads over time
Lead score distribution
Outreach performance
Conversion funnel
AI cost
AI latency


---

PHASE 28 — Production Reliability

Add:

Idempotency

Same event shouldn't send the same email twice.

Retries

1st failure → retry
2nd failure → retry
3rd failure → DLQ

Circuit breakers

If HubSpot is down:

Don't hammer HubSpot API.

Rate limiting

Respect:

LLM limits
CRM limits
Email limits

Timeouts

Every external API needs a timeout.


---

PHASE 29 — Product Grade

Now add:

Multi-tenancy
RBAC
Audit logs
API keys
Webhooks
Usage limits
Billing-ready architecture
Feature flags
Admin dashboard
Data export
GDPR-style deletion capability

Roles:

Owner
Admin
Sales Manager
Sales Rep
Viewer


---

PHASE 30 — Final Architecture

Your final system should look approximately like:

INTERNET
                            │
                         CloudFront
                            │
                           ALB
                            │
                    ┌───────▼────────┐
                    │   ECS/Fargate  │
                    │    FastAPI     │
                    └───────┬────────┘
                            │
         ┌──────────────────┼──────────────────┐
         │                  │                  │
        RDS               Redis               S3
         │
         │
       SQS
         │
 ┌───────┼──────────┐
 │       │          │
 ▼       ▼          ▼
AI    Outreach   Enrichment
Worker Worker     Worker
 │       │          │
 ▼       ▼          ▼
LLM    Email      External APIs
 │
 ├──────────────┐
 │              │
 ▼              ▼
HubSpot       Analytics


---

🧪 Final Testing

Before calling it production-ready:

□ Authentication tested
□ OAuth tested
□ Authorization tested
□ Database migrations tested
□ API tests
□ Worker tests
□ AI evaluation
□ Load testing
□ Security testing
□ Failure testing
□ Retry testing
□ DLQ testing
□ Monitoring tested
□ Backup/restore tested
□ CI/CD tested
□ Terraform tested
□ Production deployment tested


---

📅 Recommended 30-Day Execution Plan

Days	Focus

1–2	Architecture + GitHub + environment
3–4	FastAPI foundation
5–6	PostgreSQL + SQLAlchemy
7	Alembic
8–10	Google OAuth 2.0
11–12	Lead CRUD
13–15	AI lead scoring
16–17	AI personalization
18–19	SQS + workers
20	Email automation
21–22	HubSpot OAuth/API
23	Docker
24–25	Testing + AI evaluation
26	AWS infrastructure
27	Terraform
28	GitHub Actions CI/CD
29	Monitoring + security
30	Production deployment + documentation



---

🏆 What You Should Be Able to Explain After This

Don't just finish the code.

You should be able to answer:

AI Engineering

> Why structured outputs instead of parsing text?



> How do you evaluate an LLM?



> How do you control hallucinations?



> How do you reduce AI cost?



> How do you version prompts?



Automation

> Why SQS?



> What happens when a worker crashes?



> How do you prevent duplicate emails?



> How do you implement retries?



OAuth

> Authorization Code vs Client Credentials?



> What is PKCE?



> Why do we need state?



> Where should refresh tokens live?



AWS

> ECS vs Lambda?



> RDS vs DynamoDB?



> Why private subnets?



> How does ALB → ECS work?



DevOps

> How does your CI/CD pipeline work?



> How do you rollback?



> How do you manage secrets?



> How do you deploy infrastructure?



Architecture

> Why event-driven architecture?



> How would you handle 1M leads?



> Where are the bottlenecks?



> How would you make this multi-tenant?




---

🎯 Your progression

The key is not to jump directly to AWS.

Build it in this order:

LEVEL 1
Python + FastAPI
        ↓
LEVEL 2
PostgreSQL + APIs
        ↓
LEVEL 3
OAuth 2.0
        ↓
LEVEL 4
LLM + AI Engineering
        ↓
LEVEL 5
Automation + Workers
        ↓
LEVEL 6
Docker
        ↓
LEVEL 7
AWS
        ↓
LEVEL 8
Terraform
        ↓
LEVEL 9
CI/CD
        ↓
LEVEL 10
Observability + Security
        ↓
LEVEL 11
Multi-tenancy
        ↓
LEVEL 12
PRODUCT-GRADE SaaS

And one important rule: don't build this as 30 days of copy-pasting AI-generated code. Treat the AI coding agent as your junior engineer: you provide architecture and acceptance criteria, it implements, and you review/test/security-check everything. That is the skill you're actually trying to develop as an AI Automation Engineer.

# AI For Bharat — Your Intelligent Study Abroad Counsellor

> **An AI-powered platform that transforms the overwhelming study abroad journey into a guided, personalized, and stress-free experience.**

[![Status](https://img.shields.io/badge/MVP-Live%20%26%20Deployed-brightgreen?style=for-the-badge)](https://ai-counsellor-roan.vercel.app/)
[![Powered By](https://img.shields.io/badge/Powered%20By-Google%20Vertex%20AI-4285F4?style=for-the-badge&logo=google-cloud&logoColor=white)]()
[![Built With](https://img.shields.io/badge/Built%20With-TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)]()

---

### The MVP is live, deployed, and ready to test.

**[Try the Live Demo →](https://ai-counsellor-roan.vercel.app/)**

---

## The Problem

Every year, millions of students dream of studying abroad — but navigating the process is a nightmare. Researching universities, understanding fit, managing applications across multiple schools, writing SOPs, preparing for interviews, and tracking deadlines... it's a full-time job on top of being a student.

Traditional counsellors are expensive. Generic advice doesn't account for *your* unique profile. And spreadsheets can only take you so far.

## The Solution

**AI For Bharat** is your personal AI counsellor that knows you — your academics, your budget, your dreams — and guides you step by step from "I want to study abroad" to "My applications are submitted."

It doesn't just answer questions. It *thinks* with you, *plans* for you, and *acts* alongside you.

---

## What It Does

### 1. Understands You Deeply

Through a conversational onboarding flow, the AI builds a rich profile of who you are — your GPA, test scores, preferred countries, budget, work experience, and more. It then evaluates your profile strength and tells you exactly where you stand.

### 2. Recommends Universities Intelligently

No more endless Googling. The AI analyzes your profile against thousands of universities and returns personalized recommendations, categorized into:

| Category | What It Means |
|----------|--------------|
| **Dream** | Ambitious but worth the shot — competitive programs where you'd thrive |
| **Target** | Realistic matches — strong probability of admission |
| **Safe** | High acceptance likelihood — your safety net |

Each recommendation comes with a **fit score (0-100)**, detailed reasoning, risk factors, cost breakdown, acceptance probability, and clear next steps.

### 3. Manages Your Applications End-to-End

Once you shortlist and lock universities, the AI generates a **complete application strategy** for each one — including:

- Required documents checklist
- Application timeline with milestones
- AI-guided SOP writing assistance
- Exam preparation guidance
- Form submission tips
- Auto-generated task lists with priorities and deadlines

### 4. Prepares You for Interviews

A dedicated **Mock Interview** mode simulates real admissions interviews with university-specific questions, behavioral prompts, and real-time feedback on your responses.

### 5. Keeps You on Track

A unified dashboard shows your progress across all applications — what's done, what's pending, and what needs your attention right now. The AI continuously generates actionable to-dos so you never miss a beat.

---

## How It Works Under the Hood

```
  You ──→ Conversational AI Chat ──→ Profile Analysis
                                          │
                    ┌─────────────────────┤
                    ▼                     ▼
          University Discovery    Profile Evaluation
          (Vertex AI Search)      (Strength Analysis)
                    │                     │
                    ▼                     ▼
          Smart Recommendations    Personalized To-Dos
          (Dream/Target/Safe)      (What to improve)
                    │
                    ▼
          Application Strategy     Mock Interviews
          (Per University)         (Practice & Feedback)
                    │
                    ▼
          Task Management & Tracking
          (Until Submission)
```

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Runtime** | Node.js (ES Modules) |
| **Framework** | Express.js 4.x |
| **Language** | TypeScript 5.x |
| **Database** | PostgreSQL with Prisma ORM |
| **AI Engine** | Google Vertex AI — Gemini 2.5 Pro, Gemini 3 Flash |
| **University Search** | Vertex AI Search with Fuse.js fallback |
| **Authentication** | Passport.js (Email/Password + Google OAuth) |
| **Email** | SendGrid (OTP verification) |
| **Security** | JWT + Session hybrid, bcrypt, CORS, rate limiting |

### Key Architecture Decisions

- **Vertex AI over OpenAI** — Tighter Google Cloud integration, grounded search, enterprise SLAs
- **PostgreSQL over MongoDB** — Structured relational data with ACID compliance
- **AI Memory System** — Condensed context per user for efficient token usage and faster responses
- **Multi-strategy JSON Parser** — 6-layer fallback to gracefully handle any AI response format
- **Graceful Model Fallback** — Gemini 3 Flash (primary) with automatic fallback to Gemini 2.5 Pro

---

## The AI Brain

The platform features a sophisticated AI memory system that maintains a condensed understanding of each user across conversations — their profile, preferences, shortlisted universities, and current stage in the journey. This means:

- **No repetitive questions** — it remembers what you told it
- **Context-aware responses** — advice evolves as your application progresses
- **Efficient token usage** — fast, cost-effective AI interactions
- **Intelligent task routing** — profile tasks go to your dashboard, application tasks go to the right university

---

## User Journey

```
  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
  │  1. Profile   │───▶│ 2. Discover  │───▶│ 3. Shortlist │
  │  Creation     │    │  Universities│    │  & Refine    │
  └──────────────┘    └──────────────┘    └──────┬───────┘
                                                  │
                       ┌──────────────┐    ┌──────▼───────┐
                       │ 5. Submit &  │◀───│ 4. Prepare   │
                       │    Track     │    │  Applications│
                       └──────────────┘    └──────────────┘
```

**Stage 1** — Create your profile through an AI-guided conversation  
**Stage 2** — Get smart university recommendations tailored to you  
**Stage 3** — Build your shortlist, compare options, lock your targets  
**Stage 4** — AI generates strategies, tasks, and timelines per university  
**Stage 5** — Submit applications and track everything in one place  

---

## API Overview

| Domain | Endpoints | Purpose |
|--------|-----------|---------|
| **Auth** | 7 endpoints | Registration, login, OAuth, OTP verification |
| **Profile** | 3 endpoints | Profile CRUD and AI evaluation |
| **Chat** | 3 endpoints | AI counsellor, mock interviews, chat history |
| **Universities** | 2 endpoints | AI-powered search and details |
| **Shortlist** | 4 endpoints | Add, remove, view, and lock universities |
| **Strategy** | 9 endpoints | Application strategies, task management |
| **Dashboard** | 4 endpoints | Profile to-dos and progress tracking |

---

## Getting Started

### Prerequisites

- Node.js 18+
- PostgreSQL 14+
- Google Cloud project with Vertex AI enabled
- SendGrid account (for email verification)

### Setup

```bash
# Clone the repository
git clone git@github.com:maneetjha/AI-For-Bharat.git
cd AI-For-Bharat

# Install dependencies
npm install

# Set up environment variables
cp .env.example .env
# Fill in your credentials (see design.md for full list)

# Run database migrations
npx prisma migrate deploy

# Start the development server
npm run dev
```

---

## Project Structure

```
├── src/
│   ├── routes/          # Express route definitions
│   ├── controllers/     # Request handlers and business logic
│   ├── services/        # Vertex AI, search, and external integrations
│   ├── utils/           # AI memory, normalizers, evaluators, generators
│   └── prisma/          # Database schema and migrations
├── requirements.md      # Detailed functional & non-functional requirements
├── design.md            # System architecture and design document
└── README.md
```

---

## What's Next

This MVP covers the core study abroad counselling experience. On the roadmap:

- Real-time notifications and reminders
- Document upload and management
- Scholarship discovery and recommendations
- Visa guidance module
- Mobile app (React Native)
- Video interview practice with WebRTC
- Collaborative counsellor dashboard
- Payment integration for premium tiers

---

## Contributing

This project is in active development. If you'd like to contribute, feel free to open an issue or submit a pull request.

---

## License

This project is proprietary. All rights reserved.

---

<p align="center">
  <strong>Built with purpose, for every student who dares to dream beyond borders.</strong>
</p>

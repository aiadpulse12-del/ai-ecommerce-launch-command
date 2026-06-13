# 🚀 AI E-Commerce Launch Command
### A Multi-Agent AI System for Automated Product Launch Campaigns

> Built for the **Band of Agents Hackathon** — lablab.ai (June 12–19, 2026)

---

## 📌 What Is This?

**AI E-Commerce Launch Command** is a 4-agent AI system that automates the entire product launch campaign process for ecommerce brands.

Instead of hiring a market researcher, copywriter, editor, and marketing strategist separately — this system deploys 4 specialized AI agents that **collaborate with each other through Band** to deliver a complete, ready-to-launch campaign brief in minutes.

---

## 🎯 The Problem It Solves

Launching a product online requires:
- Market research (2-3 days)
- Writing copy, emails, and ad hooks (2-3 days)
- Multiple rounds of content review and revision (1-2 days)
- Building a campaign strategy with budget and KPIs (1-2 days)

**Total: 7-10 days of manual work.**

This system does it in under 5 minutes — with agents that actually talk to each other, review each other's work, and iterate until the output meets quality standards.

---

## 🤖 The 4 Agents

```
YOU (Postman / Demo UI)
        │
        ▼
┌─────────────────────────┐
│   Agent 1               │
│   RESEARCH AGENT        │
│   Analyzes market,      │
│   competitors &         │
│   positioning           │
└──────────┬──────────────┘
           │ Posts to Band + triggers
           ▼
┌─────────────────────────┐
│   Agent 2               │
│   CONTENT AGENT         │
│   Writes copy, email    │
│   sequences & ad hooks  │
└──────────┬──────────────┘
           │ Posts to Band + triggers
           ▼
┌─────────────────────────┐      ❌ Score < 7
│   Agent 3               │─────────────────────┐
│   REVIEW AGENT          │                     │
│   Scores content 1-10   │                     ▼
│   Approves or rejects   │          Sends revision notes
└──────────┬──────────────┘          back to Content Agent
           │ ✅ Score >= 7            (max 3 attempts)
           ▼
┌─────────────────────────┐
│   Agent 4               │
│   CAMPAIGN AGENT        │
│   Builds full launch    │
│   strategy & brief      │
└─────────────────────────┘
           │
           ▼
   Complete Campaign Brief
   delivered to Band room
```

---

## ⭐ Key Feature — The Feedback Loop

The **Review Agent → Content Agent feedback loop** is what makes this system genuinely collaborative rather than a simple linear pipeline.

When content scores below 7/10:
1. Review Agent posts specific revision notes to Band room tagging Content Agent
2. Content Agent rewrites the content addressing every issue
3. Review Agent scores again
4. This repeats up to 3 times until quality is approved

**This is real agent-to-agent collaboration — not just task handoff.**

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Agent Orchestration | n8n Cloud |
| AI Model | google/gemma-3-4b-it via AI/ML API |
| Collaboration Layer | Band (app.band.ai) |
| Communication Protocol | Band Agent API (HTTP POST) |
| Trigger | HTTP Webhook (Postman / Demo UI) |

---

## 🏗️ Architecture

```
Postman / Demo UI
       │
       │ POST /webhook/research-agent
       ▼
n8n Cloud (aiadpulse12.app.n8n.cloud)
       │
       ├── Agent 1: Research Agent    ──► Band Room (posts research brief)
       │                                        │
       ├── Agent 2: Content Agent    ◄──────────┘
       │   (rewrites on revision)    ──► Band Room (posts content)
       │        ▲                             │
       │        │ revision loop              │
       ├── Agent 3: Review Agent    ◄──────────┘
       │                            ──► Band Room (posts approval/rejection)
       │                                        │
       └── Agent 4: Campaign Agent  ◄──────────┘
                                    ──► Band Room (posts final brief)
```

---

## 📁 Repository Structure

```
ai-ecommerce-launch-command/
│
├── README.md
│
├── workflows/
│   ├── agent1_research_agent.json      # n8n workflow — Research Agent
│   ├── agent2_content_agent.json       # n8n workflow — Content Agent
│   ├── agent3_review_agent.json        # n8n workflow — Review Agent (with feedback loop)
│   └── agent4_campaign_agent.json      # n8n workflow — Campaign Agent
│
└── docs/
    └── architecture.png                # System architecture diagram
```

---

## 🚀 How to Run This Yourself

### Prerequisites
- n8n Cloud account (app.n8n.cloud)
- Band account (app.band.ai)
- AI/ML API key (aimlapi.com)

### Step 1 — Set Up Band
1. Create a Band account at app.band.ai
2. Create 4 agents: ResearchAgent, ContentAgent, ReviewAgent, CampaignAgent
3. Copy each agent's API key (`band_a_...`)
4. Create a shared chat room and copy the room ID

### Step 2 — Set Up n8n
1. Create n8n Cloud account at app.n8n.cloud
2. Import all 4 workflow JSON files from the `workflows/` folder
3. In each workflow, update:
   - AI/ML API key in the OpenAI HTTP node
   - Band agent API key in the Build Message node
   - Band chat room ID (already set as default in workflows)

### Step 3 — Activate All Workflows
In n8n, toggle **Active ON** for all 4 workflows.

### Step 4 — Trigger the Pipeline
Send a POST request to your Research Agent webhook:

```bash
curl -X POST https://YOUR-N8N-INSTANCE.app.n8n.cloud/webhook/research-agent \
  -H "Content-Type: application/json" \
  -d '{
    "product_name": "NightOwl Blue Light Glasses",
    "product_description": "Premium blue light blocking glasses for digital eye strain relief",
    "target_audience": "Remote workers and gamers aged 22-40",
    "chat_id": "YOUR-BAND-CHAT-ROOM-ID"
  }'
```

### Step 5 — Watch the Agents Collaborate
Open your Band chat room and watch all 4 agents post messages in real time.

---

## 📊 What the Pipeline Produces

### Research Brief
- Market positioning
- Competitor angles
- Customer pain points
- Unique selling points
- Recommended channels
- Urgency score (1-10)

### Content Package
- Product headline, subheadline, body copy, CTA
- 3-email launch sequence (subject, preview, body)
- 5 high-converting ad hooks

### Campaign Brief
- Campaign name
- 3-phase launch timeline
- Channel strategy with budget split and expected ROAS
- KPIs with targets
- Launch checklist
- Executive summary
- Confidence score (1-10)

---

## 💬 What the Band Room Looks Like

```
ResearchAgent:  @ContentAgent 🔍 RESEARCH COMPLETE
                Product: NightOwl Blue Light Glasses
                Market Positioning: Premium eye protection...
                ACTION: Write copy, emails, and ad hooks.

ContentAgent:   @ReviewAgent ✍️ CONTENT READY (Attempt 1/3)
                Headline: See Clearly. Sleep Deeply.
                ...
                ACTION: Score this content.

ReviewAgent:    @ContentAgent ❌ REVISION REQUIRED (Attempt 1/3)
                Score: 5/10
                Issues: Headline lacks urgency, CTA is weak...
                ACTION: Fix and resubmit.

ContentAgent:   @ReviewAgent ✍️ CONTENT READY (Attempt 2/3)
                Headline: Stop Eye Strain Before It Stops You.
                ...

ReviewAgent:    @CampaignAgent ✅ CONTENT APPROVED
                Score: 8/10
                ACTION: Build the campaign brief.

CampaignAgent:  🎉 PIPELINE COMPLETE
                Campaign: NightOwl Vision Launch
                Confidence: 9/10
                Executive Summary: ...
```

---

## 🏆 Hackathon Track

**Track 1: Internal Enterprise Workflows**

This system demonstrates how enterprise ecommerce teams can replace days of manual coordination between departments with an automated multi-agent pipeline where every handoff, review, and revision happens through Band.

---

## 👨‍💻 Built By

**Ahmad Raza**
Founder, AI AdPulse
AI Automation Agency — Lahore, Pakistan

- Portfolio: ahmadraza-ai.me
- Agency: AI AdPulse



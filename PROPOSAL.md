# Training Proposal

## Building Retrieval-Augmented Generation (RAG) & Agentic AI Applications with n8n

### Prepared for: Jabatan Pengangkutan Jalan Malaysia (JPJ)

---

| | |
|---|---|
| **Programme Title** | Building Retrieval-Augmented Generation (RAG) Applications with n8n |
| **Prepared For** | Jabatan Pengangkutan Jalan Malaysia (JPJ) |
| **Duration** | 3 Days (21 Hours) |
| **Level** | Beginner |
| **Training Mode** | Physical / Virtual / Hybrid |
| **Course Code** | RAG-N8N-JPJ-101 |
| **Proposal Date** | 16 June 2026 |
| **Version** | 1.0 (2026) |

---

## 1. Executive Summary

Jabatan Pengangkutan Jalan (JPJ) manages a vast and continuously evolving body of
knowledge — road transport regulations (Akta Pengangkutan Jalan 1987), licensing
procedures, vehicle registration rules, summons and compound guidelines, standard
operating procedures (SOPs), and public service circulars (Pekeliling). Frontline
officers, counter staff, and the public frequently need fast, accurate answers drawn
from these documents.

This 3-day hands-on programme equips JPJ officers to build **AI-powered knowledge
assistants** using **Retrieval-Augmented Generation (RAG)** and the **n8n** workflow
automation platform — *without extensive coding*. By the end of the programme, each
participant will have built a working RAG assistant grounded in JPJ's own documents,
which can be adapted for internal helpdesk use, counter support, or public-facing
enquiry services.

The programme combines theory, live demonstrations, guided labs, and a project-based
capstone, ensuring participants leave with a deployable solution and the confidence
to maintain and extend it.

---

## 2. Why This Matters for JPJ

| Challenge at JPJ | How a RAG Assistant Helps |
|---|---|
| Officers spend time searching circulars, acts, and SOP manuals | Instant, source-grounded answers in seconds |
| Inconsistent answers across counters and branches | A single authoritative knowledge base ensures consistency |
| New staff onboarding takes time to learn procedures | An assistant that explains procedures on demand |
| High volume of repetitive public enquiries (licensing, renewal, summons) | Self-service assistant reduces counter load |
| Knowledge is locked in PDFs and scattered documents | Documents become searchable, conversational knowledge |
| Concern over AI "making things up" (hallucination) | RAG answers are grounded in and cite official documents |

> **Data residency & governance note:** The recommended stack can run entirely on-premise
> or on a government-approved VPS/cloud, with the option to use **Ollama** (local LLM) so
> that sensitive documents never leave JPJ-controlled infrastructure. This is covered
> explicitly on Day 3.

---

## 3. Programme Overview

This intensive 3-day hands-on training introduces participants to modern AI application
development using Retrieval-Augmented Generation (RAG) and n8n.

Participants will learn how to build AI-powered assistants that answer questions based on
organisational documents, policies, manuals, and knowledge bases — with minimal coding.
All exercises use **JPJ-relevant sample documents** so the learning is immediately
applicable to the department's real-world needs.

---

## 4. Learning Outcomes

Upon completion, participants will be able to:

- Understand Generative AI and Large Language Models (LLMs)
- Explain the concepts behind RAG architectures
- Understand embeddings and vector databases
- Build AI workflows using n8n
- Create document ingestion pipelines for JPJ documents (acts, circulars, SOPs)
- Store and retrieve knowledge using vector databases
- Build AI-powered chatbots and assistants
- Deploy and maintain a production-ready RAG solution
- Implement best practices for AI governance, data security, and accuracy

---

## 5. Prerequisites

- Basic computer literacy
- Familiarity with web applications
- **No programming experience required**
- Laptop with internet connection

---

## 6. Detailed Curriculum

### DAY 1 — Foundations of AI, LLMs and RAG
**Duration: 7 Hours**

#### Session 1 — Introduction to Artificial Intelligence
**Topics Covered**
- Evolution of Artificial Intelligence
- Machine Learning vs Generative AI
- Current AI Landscape
- AI Adoption in the Public Sector & Government Services
- AI Opportunities in Road Transport & Licensing

**Learning Activity — Workshop Discussion:** Identify potential AI use cases within JPJ
(e.g., licensing enquiries, summons clarification, internal SOP lookup).

---

#### Session 2 — Understanding Large Language Models (LLMs)
**Topics Covered**
- What are LLMs?
- GPT, Claude, Gemini and Open Source Models (incl. local models via Ollama)
- Tokens and Context Windows
- Prompt Engineering Fundamentals
- AI Hallucinations and Limitations

**Practical Exercise — Prompt Engineering Workshop:** Compare prompting techniques and
evaluate AI responses using JPJ-style enquiry questions.

---

#### Session 3 — Understanding Retrieval-Augmented Generation (RAG)
**Topics Covered**
- Why Traditional LLMs Are Not Enough
- Knowledge Limitations of AI Models
- Benefits of RAG (accuracy, citations, up-to-date knowledge)
- Public Sector RAG Use Cases

**RAG Architecture Overview**
```
User Question
      ↓
  Embedding
      ↓
Vector Search
      ↓
Relevant Context
      ↓
     LLM
      ↓
   Answer
```

**Group Activity:** Design a knowledge assistant for your division (e.g., Licensing,
Enforcement, Registration).

---

#### Session 4 — Embeddings and Vector Databases
**Topics Covered**
- What are Embeddings?
- Semantic Search Concepts
- Similarity Search & Cosine Similarity
- Vector Indexing

**Introduction to Vector Databases**
- Qdrant
- Pinecone
- Weaviate
- Chroma
- PostgreSQL + pgvector

**Demonstration:** Visualising semantic search using sample datasets.

---

#### Session 5 — Introduction to n8n
**Topics Covered**
- What is n8n?
- Workflow Automation Concepts
- Nodes and Connections
- Credentials Management
- AI Nodes Overview

**Hands-On Lab — Build Your First AI Workflow**
```
Webhook
   ↓
OpenAI
   ↓
Response
```

**Deliverable:** Participants create their first AI-powered API endpoint.

---

### DAY 2 — Building a Complete RAG Solution
**Duration: 7 Hours**

#### Session 6 — RAG Architecture Deep Dive
**Topics Covered**
- Document Ingestion Pipeline
- Embedding Generation
- Retrieval Process
- Context Injection
- Response Generation

**Architecture Walkthrough**
```
PDF
 ↓
Text Extraction
 ↓
Chunking
 ↓
Embedding
 ↓
Vector Database
```

---

#### Session 7 — Setting Up a Vector Database
**Topics Covered**
- Introduction to Qdrant
- Collections and Vectors
- Metadata Storage
- Search Optimisation

**Hands-On Lab — Deploy Qdrant using Docker:** Participants create their own vector
database environment.

---

#### Session 8 — Document Processing and Chunking
**Topics Covered**
- PDF Extraction
- Text Cleaning
- Chunking Strategies
- Metadata Design

**Best Practices**
- Chunk Size Selection
- Overlapping Chunks
- Document Organisation

**Practical Exercise:** Import JPJ circulars, SOPs, and policy documents.

---

#### Session 9 — Building the Ingestion Workflow in n8n
**Workflow Design**
```
File Upload
      ↓
Extract Text
      ↓
Chunk Content
      ↓
Generate Embeddings
      ↓
Store in Qdrant
```

**Hands-On Lab:** Participants build a complete document indexing workflow.

---

#### Session 10 — Building the Retrieval Workflow
**Workflow Design**
```
User Question
      ↓
  Embedding
      ↓
Vector Search
      ↓
Retrieve Context
      ↓
     LLM
      ↓
   Answer
```

**Hands-On Lab — Build a JPJ Knowledge Assistant**

---

#### Day 2 Project — JPJ Licensing & Procedures Assistant
Participants will create an assistant capable of answering questions from:
- Driving Licence (LMM/CDL/GDL) procedures
- Vehicle Registration & Ownership Transfer guidelines
- Summons & Compound (saman) procedures
- Internal SOP Documents

**Example Questions:**
- *What are the requirements to renew a Competent Driving Licence (CDL)?*
- *What is the procedure for ownership transfer of a vehicle?*
- *How does a member of the public settle an outstanding compound?*

---

### DAY 3 — AI Agents, Optimisation and Deployment
**Duration: 7 Hours**

#### Session 11 — AI Agents with n8n
**Topics Covered**
- What is an AI Agent?
- Agent vs Workflow
- Tool Calling Concepts
- Agent Reasoning

**Agent Architecture**
```
User
 ↓
AI Agent
 ↓
Select Tool
 ↓
Execute Tool
 ↓
Generate Answer
```

**Demonstration:** Building a simple AI agent.

---

#### Session 12 — Multi-Tool AI Agents
**Topics Covered**
- Connecting Multiple Tools
- Dynamic Tool Selection
- API Integration
- Database Integration

**Hands-On Lab — Build a JPJ Service Agent**

Capabilities:
- Search Knowledge Base (acts, circulars, SOPs)
- Look Up Licence / Registration Status (mock API)
- Retrieve Applicant Information
- Create Service / Support Tickets

---

#### Session 13 — Advanced RAG Techniques
**Topics Covered**
- Metadata Filtering (e.g., by document type or division)
- Hybrid Search
- Re-Ranking
- Parent-Child Retrieval
- Multi-Document Retrieval

**Demonstration:** Comparing retrieval quality across methods.

---

#### Session 14 — RAG Evaluation and Optimisation
**Topics Covered**
- Measuring Accuracy
- Reducing Hallucinations
- Prompt Optimisation
- Retrieval Improvements
- Cost Optimisation

**Practical Exercise:** Improve answer quality through iterative testing.

---

#### Session 15 — Production Deployment
**Production Architecture**
```
Users
   ↓
Web Application
   ↓
   n8n
   ↓
OpenAI / Ollama
   ↓
 Qdrant
   ↓
PostgreSQL
```

**Topics Covered**
- Docker Deployment
- VPS / On-Premise Hosting (for data residency)
- Cloud Deployment
- Security Best Practices
- Monitoring and Logging

---

## 7. Final Capstone Project

### Build a Production-Ready Knowledge Assistant

Participants will design and implement a complete RAG application. **Suggested options
tailored to JPJ:**

| Option | Project |
|---|---|
| 1 | JPJ Licensing Assistant (driving licence enquiries & procedures) |
| 2 | Vehicle Registration & Ownership Transfer Assistant |
| 3 | Enforcement & Compound (Saman) Information Assistant |
| 4 | Internal SOP & Circular (Pekeliling) Helpdesk for officers |
| 5 | Public Counter Service Knowledge Base |

---

## 8. Assessment Criteria

| Criteria | Weightage |
|---|---|
| Workflow Design | 20% |
| Document Ingestion | 20% |
| Retrieval Accuracy | 20% |
| Chatbot Functionality | 20% |
| Presentation & Documentation | 20% |

---

## 9. Recommended Software Stack

| Component | Technology |
|---|---|
| Workflow Automation | n8n |
| LLM Provider | OpenAI |
| Alternative LLM (on-premise / local) | Ollama |
| Vector Database | Qdrant |
| Database | PostgreSQL |
| File Storage | MinIO |
| Deployment | Docker |
| Monitoring | Grafana |
| Reverse Proxy | Traefik |

> For JPJ deployments handling sensitive data, the **Ollama + on-premise** configuration
> is recommended to keep all documents within JPJ-controlled infrastructure.

---

## 10. Training Deliverables

**Training Materials**
- Training Slides
- Workshop Manual
- Lab Guide
- Architecture Reference

**Templates**
- RAG Workflow Template
- Document Ingestion Template
- AI Agent Workflow Template
- Deployment Template

**Sample Data (JPJ-contextualised)**
- Sample Driving Licence procedures
- Sample SOP Documents
- Sample FAQ Documents
- Knowledge Base Examples

**Source Files**
- n8n Workflow JSON Files
- Docker Deployment Files
- Prompt Templates

---

## 11. Certification Requirements

Participants must successfully complete:
- All practical exercises
- Day 2 RAG implementation
- Final capstone project
- Project presentation

Upon successful completion, participants will receive a:

> **Certificate of Completion**
> *Building Retrieval-Augmented Generation (RAG) Applications with n8n*

---

## 12. Target Audience (JPJ)

- IT Officers (Pegawai Teknologi Maklumat)
- System Administrators
- Business Analysts
- Developers
- Digital Transformation Teams
- Knowledge Management Teams
- Counter & Service Officers (as power users / testers)
- AI Enthusiasts within the department

---

## 13. Logistics & Requirements

**Venue & Mode:** Physical / Virtual / Hybrid (to be confirmed with JPJ)

**Per-Participant Requirements:**
- Laptop (min. 8GB RAM recommended) with admin rights to install Docker
- Stable internet connection
- Modern web browser

**Recommended Class Size:** 15–25 participants (to ensure hands-on attention)

**Facilities (physical mode):** Training room with projector, power points, and Wi-Fi.

---

## 14. Course Information Summary

| | |
|---|---|
| **Course Code** | RAG-N8N-JPJ-101 |
| **Training Duration** | 21 Hours (3 Days) |
| **Level** | Beginner |
| **Version** | 1.0 (2026) |

---

*This proposal is prepared specifically for Jabatan Pengangkutan Jalan Malaysia (JPJ).
Sample documents, capstone scenarios, and example questions can be further customised to
match JPJ's actual circulars, SOPs, and service workflows upon engagement.*

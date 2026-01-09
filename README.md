# LocalHelp — Community Services Finder

Author: Emmanuel Tuyizere (@emmanueltuyizere)

One-liner
A simple conversational assistant that helps people quickly find nearby community services (health clinics, legal aid, shelters, social support) and gives clear next steps and contact details.

Background
- Problem statement: Many people (low-income families, newcomers, displaced persons) struggle to locate the right local services quickly. Information is scattered, written in technical language, or out of date.
- Frequency: This is a common, recurring problem in cities and rural areas worldwide—especially where official guidance is sparse or not centralized.
- Personal motivation: I want to reduce friction for people who need essential help and make it easier for service providers to reach those who need them.
- Why it matters: Faster, clearer access to services can prevent harm, improve outcomes, and connect people with critical support faster.

Data and AI techniques
- Data sources
  - Public directories (municipal open data, NGO lists), OpenStreetMap for POIs.
  - Curated CSV/JSON of services: name, category, address, lat/lon, contact, hours, eligibility, short description.
  - Optional opt‑in anonymized usage logs to improve ranking and phrasing.
- Data format & minimum fields
  - service_name, category, address, latitude, longitude, phone, email, hours, languages, eligibility_notes, description, source_url, last_verified.
- AI techniques
  - Embeddings + vector search (sentence-transformers → FAISS/Weaviate) to match free-text queries to service descriptions.
  - Small LLM (hosted or API) for intent extraction, friendly phrasing, and producing concise next-step instructions.
  - Geospatial filtering (distance and administrative area) and simple rule-based checks for “open now”, language match, eligibility.
  - Human-in-the-loop verification for critical entries (legal/medical) and crowdsourced claiming of entries by providers.
- Design principle: Use LLMs for language and intent handling but rely on the database for factual service metadata (addresses, phones).

How it is used
- Target users: community members seeking immediate help; social workers and volunteers.
- Typical flow:
  1. User types or speaks a request in a web chat or SMS (e.g., “I need a shelter that accepts families with pets and is within 10 km”).
  2. System extracts filters (category, languages, constraints) and applies geospatial filtering.
  3. Vector search retrieves relevant entries; results are ranked by relevance + distance.
  4. Assistant returns 1–3 top matches with concise action items: address, phone, hours, eligibility, and “next step” (e.g., “Call first to confirm bed availability”).
  5. User can ask follow-ups (transport options, documents required).
- Accessibility: Plain-language UI, local language support, SMS fallback for low-data users, and an anonymous mode.

Challenges and limitations
- Data freshness: Hours and contacts change; the system is a triage helper, not a guaranteed live directory. Mitigation: show source and last_verified timestamp; encourage calls before arrival.
- Hallucination: LLMs can invent details. Mitigation: never generate factual metadata (phone/address) — only present database fields; use LLMs only to summarize and phrase.
- Coverage bias: Informal or new providers may be missing. Mitigation: provide a verified submission workflow and outreach to local NGOs.
- Privacy and safety: Users may share sensitive data. Store minimal data, encrypt logs, require explicit consent for any retention, and provide clear disclaimers.
- Legal limits: This is a signposting tool, not professional medical or legal advice. Include clear disclaimers and escalation paths.

What next (roadmap)
- MVP (1–2 weeks)
  - Curate 100–300 service entries for one town/district.
  - Build a minimal backend: query → embeddings → vector search → return top 3.
  - Add a simple web chat UI and an SMS option (Twilio).
  - Show "call to verify" notice and data source for each result.
- Pilot (1–2 months)
  - Partner with a local NGO or community center; run an opt‑in pilot; collect anonymous feedback.
  - Add a provider claim/update dashboard and simple moderation.
- Scale (3–9 months)
  - Multi-language voice support, PWA offline-first client, scheduled re-checks of listings, and partnerships with municipal open-data teams.
  - Funding and sustainability via grants or local partnerships.

Acknowledgments and inspirations
- OpenStreetMap (OSM) for POI data.
- Sentence-transformers / FAISS, LangChain (for orchestration ideas), and civic tech directories that inspired this project.
- Elements of AI course — this README is prepared as the course final project submission.

How to submit this as your final project README
- This file is formatted as a ready-to-use README.md for your GitHub repository.
- If you want, I can:
  - Create the GitHub repository for you and push this README (I will ask for repo name and confirmation before creating anything).
  - Or produce a downloadable ZIP containing README.md, LICENSE (MIT), and a basic CONTRIBUTING.md.

Optional: How to run a minimal demo (technical notes)
- Suggested tech (beginner-friendly):
  - Backend: Python + FastAPI
  - Embeddings: sentence-transformers (all-MiniLM) → FAISS
  - LLM: OpenAI (chat completion) or a small local model
  - Frontend: static HTML/JS chat or SMS via Twilio
- High-level steps:
  1. Prepare CSV of services and compute embeddings for descriptions.
  2. Load embeddings into FAISS and expose a search API.
  3. Accept user query → embed → FAISS search → return top K.
  4. Use LLM only to rephrase retrieved entries and provide next‑step guidance.
  5. Deploy backend on a small cloud VM or serverless function; host frontend on Netlify/Vercel or GitHub Pages.

License
- MIT License. Include a LICENSE file when you create the repo.

Contact / Next actions
- I can do one of the following next:
  A) Create a GitHub repo under your account named `localhelp` and push this README + LICENSE + CONTRIBUTING.md. (I will ask for your confirmation before creating.)
  B) Produce a ZIP with these files you can upload yourself.
  C) Scaffold a minimal demo (FastAPI + example data) and give step-by-step run instructions.
- Reply with A, B, or C and any details (confirm repo name `localhelp` or give another name). If you choose A, I will ask one confirmation step before creating the repo.

# Interview Preparation Guide — Abhishek Jagtap (v2)

> Built strictly from the resume content provided. Every question is traceable to a specific line, claim, or technology in the resume. Where the resume does not give enough detail for a truthful answer, the answer is marked `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` with guidance on what to prepare.

---

## Resume Interview Triggers

| # | Resume Statement / Trigger | Where | Why an Interviewer May Explore It | Possible Follow-up Areas |
|---|---|---|---|---|
| 1 | "4+ years of software engineering experience and 2+ years of hands-on experience in Generative AI and Microsoft Azure" | Summary | Establishes seniority mix vs. actual GenAI depth | Timeline of GenAI ramp-up, what pre-GenAI work looked like |
| 2 | "building AI-powered enterprise solutions using Azure OpenAI, RAG, Azure AI Search, Azure AI services, and Python" | Summary | Core positioning claim — must be able to back with real project detail | Which project used which service, personal role in each |
| 3 | "backed by strong expertise in React, .NET, cloud-native applications, microservices, and distributed systems" | Summary | Interviewer will check if this is genuine depth or resume padding | Concrete microservices/distributed-systems examples |
| 4 | "Proven ability to design and deliver scalable, production-ready AI applications" | Summary | Strong claim — interviewer will ask for a specific example of "designed" | Which project actually involved design ownership (DIP pipeline) |
| 5 | GenAI for insurance claims processing and document understanding (GreenShield) | IBM / GreenShield | Direct, current GenAI production experience — highest interview focus | RAG pipeline details, extraction accuracy, model/prompt choice |
| 6 | "Integrated GenAI capabilities into enterprise insurance workflows in collaboration with engineering and business teams" | GreenShield | Tests how business requirements were translated into an AI workflow | What business teams needed, how requirements shaped the AI design |
| 7 | AI-powered document intelligence platform for insurance providers (DIP) | Neudesic / DIP | Most detailed and strongest GenAI project on resume | End-to-end pipeline, LLM integration, document types |
| 8 | "Led the migration from Azure Storage Queues to Azure Service Bus, improving scalability and reliability while enabling parallel processing across multiple business units" | DIP | Strongest ownership/leadership claim on the resume | What "led" meant personally, before/after scalability difference |
| 9 | "Designed and implemented scalable document processing pipelines using Azure AI services" | DIP | Explicit design ownership claim | Actual architecture, failure handling, why those design choices |
| 10 | "Integrated LLM-based APIs to enhance document understanding" | DIP | Direct LLM integration work | Which APIs, prompt design, error handling |
| 11 | Azure AI Document Intelligence + Azure Functions listed among DIP technologies | DIP | Tests specific service-level understanding beyond generic "Azure AI services" | How Document Intelligence and Functions fit into the pipeline |
| 12 | Healthcare staffing platform, Notifications Team, multi-channel communication | Neudesic / Healthcare Staffing | Earlier, non-GenAI project — tests breadth before AI focus | Frontend/backend split, real-time notification mechanics |
| 13 | "Integrated Azure AI Search to enhance search capabilities across communication logs" | Healthcare Staffing | Early exposure to Azure AI Search, before GenAI-focused roles | Why search was needed, indexing approach |
| 14 | CDK Global internship, full-stack features, full-time offer | CDK Global | Career origin story, growth narrative | What led to full-time conversion, biggest learning |
| 15 | Career progression: Intern → Consultant → Senior Consultant (Neudesic, an IBM Company) → Senior Application Consultant (IBM) | Full resume | Rapid title progression, and Neudesic/IBM relationship | Clarify acquisition/transition, what changed in scope of responsibility |
| 16 | GreenShield project (Dec 2025–Jun 2026) listed under IBM heading, but IBM role itself starts Feb 2026 | IBM section | Minor date overlap an attentive interviewer might notice | Clarify transition period between Neudesic and IBM |
| 17 | Skills listed without explicit project narrative: LangChain, LangGraph, AI Agents, Function Calling, LLM Evaluation, Pydantic, Vector Search, Docker | Core Skills | Interviewer will probe depth vs. exposure | Where/how these were actually used |
| 18 | Certifications: Azure AI Apps and Agents Developer Associate, IBM Generative & Agentic AI Developer/Foundation | Certifications | Validates AI focus, may trigger conceptual questions | Agentic AI concepts, Azure AI Foundry familiarity |
| 19 | Cross-stack pattern: Python + React + Node.js + .NET on the same platform (DIP) | DIP | Tests understanding of how these pieces integrate | Data flow between services, API contracts |
| 20 | Azure OpenAI usage repeated across GreenShield and DIP | GreenShield / DIP | Tests real, consistent GenAI depth vs. one-off exposure | Model/prompt choices, how usage differed between the two projects |

---

## Part 1: Role & Project Deep-Dive Questions

### A. GreenShield — Generative AI / Insurance (IBM, Dec 2025–Jun 2026, current project)

---

### Question 1
**Question:** Tell me about the GreenShield project — what were you building and for what purpose?

**Model Answer:**
GreenShield was focused on developing and integrating Generative AI capabilities for insurance claims processing and document understanding. Insurance claims involve a lot of unstructured data — documents that aren't in a consistent format — so the goal was to use GenAI, along with Azure AI services, to process and extract information from that unstructured data, which then fed into downstream claims processing workflows. I worked with Azure OpenAI, Python, and RAG as part of building that capability.

**Key Points to Remember:**
- Domain: insurance claims processing, document understanding.
- Core tech: Azure OpenAI, Python, RAG, Azure AI services.
- Output feeds "downstream claims processing" — part of a bigger workflow, not the whole system.

**Likely Follow-up Questions:**
- **Follow-up Question:** What kind of unstructured insurance data are we talking about — scanned PDFs, forms, free text?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't specify document types; I should be ready to describe the actual formats (e.g., claim forms, scanned documents, emails) I worked with.
- **Follow-up Question:** How is GreenShield different from the DIP project at Neudesic, which sounds similar?
  **Model Follow-up Answer:** They're both in the insurance/document-processing space and both use GenAI and Azure AI services, but they were separate engagements at different companies — DIP was the more architecturally involved one where I helped design pipelines and led a messaging migration; GreenShield was more about developing and integrating GenAI capabilities directly into the claims workflow.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Developed and integrated Generative AI capabilities for insurance claims processing and document understanding."
**Why an Interviewer May Ask This:** To understand the second GenAI-focused project and how it compares to DIP.
**Experience Type:** Directly supported by my resume.

---

### Question 2
**Question:** How did you use RAG specifically in the GreenShield project?

**Model Answer:**
RAG was used to ground the GenAI output in the actual content of the insurance documents rather than relying purely on a model's general knowledge — so instead of just asking a model to extract information, we'd retrieve the relevant parts of the document or knowledge base and provide that as context before generating a response or extraction. This helps make the extraction more accurate and traceable back to the source document, which matters a lot in an insurance claims context where accuracy is important.

**Key Points to Remember:**
- RAG = retrieval + generation, grounding output in actual document content.
- Rationale: accuracy and traceability matter for insurance claims.
- Keep it conceptually correct without inventing exact retrieval architecture details not in the resume.

**Likely Follow-up Questions:**
- **Follow-up Question:** What did you use for the retrieval/vector store component — Azure AI Search?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — Azure AI Search is listed among the technologies for GreenShield, so it's reasonable that it played the retrieval role, but I should confirm and describe the actual setup I worked with.
- **Follow-up Question:** How did you evaluate whether the RAG-based extraction was accurate?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't describe an evaluation methodology; I should prepare a real answer about how output quality/accuracy was checked (manual review, sample validation, etc.).

**Resume Reference and Interview Context:**
**Resume Trigger:** "Worked with Azure OpenAI, Python, RAG, and Azure AI services to process and extract information from unstructured insurance data."
**Why an Interviewer May Ask This:** RAG is a headline skill on the resume — interviewer will check for real, working-level understanding.
**Experience Type:** Directly supported by my resume for the "what/why"; conceptual elaboration for the "how."

---

### Question 3
**Question:** What challenges come with extracting information from unstructured insurance documents using GenAI?

**Model Answer:**
The big challenge is that insurance documents vary a lot in structure and quality — different formats, different terminology, sometimes scanned or poorly formatted content — so getting consistent, accurate extraction isn't trivial. Part of the work is making sure the document understanding and extraction step handles that variability well before the data moves into downstream claims processing, since any errors there propagate forward into decisions that depend on that data.

**Key Points to Remember:**
- Core challenge: variability and inconsistency in unstructured document formats.
- Errors propagate downstream into claims processing — stakes matter.
- Keep answer conceptual/general since resume doesn't give a specific failure story.

**Likely Follow-up Questions:**
- **Follow-up Question:** Did you encounter a specific case where extraction failed or was inaccurate? How was it handled?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I need a real example here; the resume doesn't document a specific failure or fix.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Contributed to AI-powered workflows for document understanding, information extraction, and downstream claims processing."
**Why an Interviewer May Ask This:** To probe realistic engineering challenges rather than an idealized description.
**Experience Type:** Conceptual knowledge connected to my resume; specific incident is a preparation gap.

---

### Question 4
**Question:** What Azure OpenAI model or prompting approach did you use to extract claim data from these documents?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — my resume states I "worked with Azure OpenAI, Python, RAG, and Azure AI services to process and extract information from unstructured insurance data," but it doesn't name a specific model or prompting technique. I should be ready to describe the actual model (e.g., a GPT-family model available through Azure OpenAI) and the prompting approach I used — for example, whether I used structured/JSON-output prompting so extracted claim data could be reliably parsed downstream.

**What the interviewer is likely expecting:** A specific, credible technical answer about model choice and prompt design, not just "we used Azure OpenAI."
**What details I should prepare:** The model used, why it was appropriate for this task, and how I structured prompts to get consistent, parseable extraction output.

**Key Points to Remember:**
- Don't just say "Azure OpenAI" — be ready with the actual model and prompting pattern.
- Tie the answer back to the goal: consistent, structured extraction of claim data.

**Likely Follow-up Questions:**
- **Follow-up Question:** Did you use any structured output or schema validation (e.g., Pydantic) to enforce the format of extracted data?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — Pydantic is listed in my skills; I should confirm whether I actually used it here to validate LLM output against a schema.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Worked with Azure OpenAI, Python, RAG, and Azure AI services to process and extract information from unstructured insurance data."
**Why an Interviewer May Ask This:** To go one level deeper than the resume's general phrasing and test real GenAI implementation knowledge.
**Experience Type:** Directly supported by my resume for the "what"; model/prompt specifics need personal preparation.

---

### Question 5
**Question:** The resume says you integrated GenAI capabilities "in collaboration with engineering and business teams." What did that collaboration actually look like?

**Model Answer:**
Working with business teams meant understanding what data points actually mattered for claims processing from their perspective — what fields needed to be extracted accurately, what counted as an acceptable result, and how the AI-powered output would plug into their existing claims workflow. On the engineering side, it was more about how that GenAI capability integrated technically into the broader system. So my role bridged translating those business requirements into something the AI workflow could actually be built and tuned around, rather than working from a purely technical spec in isolation.

**Key Points to Remember:**
- Business collaboration = understanding what claim data mattered and what "good" output looked like from a business perspective.
- Engineering collaboration = technical integration into the broader system.
- Shows requirements-gathering and cross-functional skills, not just coding.

**Likely Follow-up Questions:**
- **Follow-up Question:** Can you give an example of a business requirement that changed how you approached the AI workflow?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should recall a real instance where business feedback changed an extraction rule, a prompt, or how output was structured.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Integrated GenAI capabilities into enterprise insurance workflows in collaboration with engineering and business teams."
**Why an Interviewer May Ask This:** To assess cross-functional collaboration skills alongside technical contribution.
**Experience Type:** Directly supported by my resume; specific example needs personal preparation.

---

### B. DIP — Document Intelligence Platform (Neudesic, Feb 2025–Dec 2025)

---

### Question 6
**Question:** Tell me about the Document Intelligence Platform — what did it do and who used it?

**Model Answer:**
DIP was an AI-powered document intelligence platform used by insurance providers to process claims and extract structured data from large volumes of unstructured documents. It used Generative AI and Azure AI services to automate document classification, data extraction, and the downstream processing workflows that depend on that data. This was probably my most architecturally involved GenAI project — beyond just integrating AI capabilities, I also worked on the pipeline design and a messaging infrastructure migration for it.

**Key Points to Remember:**
- Users: insurance providers; purpose: claims processing, structured data extraction.
- Core capabilities: document classification, data extraction, downstream processing.
- This is the resume's strongest/most detailed GenAI project — expect deep questions here.

**Likely Follow-up Questions:**
- **Follow-up Question:** How large a volume of documents are we talking about, roughly?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume says "large volumes" but no number; I should have a realistic sense of scale to describe if asked.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Worked on an AI-powered document intelligence platform used by insurance providers to process claims and extract structured data from large volumes of unstructured documents."
**Why an Interviewer May Ask This:** Establishing project context before drilling into specific contributions.
**Experience Type:** Directly supported by my resume.

---

### Question 7
**Question:** You mention you "led the migration from Azure Storage Queues to Azure Service Bus" — walk me through that. Why was the change needed, and what did leading it involve?

**Model Answer:**
The platform was originally using Azure Storage Queues for message handling, but as the volume and complexity of document processing grew — especially needing to support multiple business units processing documents in parallel — Storage Queues had limitations around things like more advanced messaging patterns and scalability compared to Service Bus. I led that migration, which meant figuring out how the existing queue-based processing logic needed to change to work with Service Bus, making that change across the pipeline, and validating that it actually improved scalability and reliability, and specifically that it enabled parallel processing across the different business units using the platform.

**Key Points to Remember:**
- Storage Queues → Service Bus driven by scalability/reliability needs and parallel processing across business units.
- "Led" — this is the strongest ownership claim on the resume; own it confidently but stay factual.
- Outcome: improved scalability and reliability, enabled parallel processing across business units.

**Likely Follow-up Questions:**
- **Follow-up Question:** What specifically does Service Bus offer that Storage Queues doesn't, that mattered here?
  **Model Follow-up Answer:** Service Bus supports more advanced messaging capabilities — things like topics and subscriptions for pub/sub patterns, better support for ordering and sessions, and more robust handling for higher-throughput, multi-consumer scenarios — which fit better with needing multiple business units to process documents in parallel rather than everything going through one basic queue.
- **Follow-up Question:** How did you handle the cutover — was there a period where both systems ran, and how did you avoid message loss?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't describe the cutover mechanics; I should prepare a real answer about how the transition was managed (parallel run, phased rollout, etc.) and how message integrity was ensured.
- **Follow-up Question:** What would you say was the actual measured improvement in reliability or throughput?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — no specific metric is on the resume. I should avoid inventing a number and instead describe the improvement qualitatively unless I have a real figure to cite.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Led the migration from Azure Storage Queues to Azure Service Bus, improving scalability and reliability while enabling parallel processing across multiple business units."
**Why an Interviewer May Ask This:** It's the single strongest leadership/ownership claim on the resume, and interviewers will want to validate it in depth.
**Experience Type:** Directly supported by my resume; specific mechanics/metrics need personal preparation.

---

### Question 8
**Question:** The resume says you "designed and implemented scalable document processing pipelines using Azure AI services." What did that design work actually involve?

**Model Answer:**
The pipeline needed to take in large volumes of claim-related documents, run them through classification and extraction steps using Azure AI services, and feed structured output downstream — and it needed to do that reliably at scale rather than one document at a time in a fragile way. My design work was about structuring that flow so it could handle volume and be resilient — for example, thinking through how documents move through each processing stage, how failures at one stage are handled without blocking everything else, and integrating the Azure AI services and the messaging layer (Service Bus) into that flow.

**Key Points to Remember:**
- Scope: pipeline design for classification + extraction + downstream structured output at scale.
- Ties directly to the Service Bus migration — messaging and pipeline design are connected.
- Emphasize resilience/scalability thinking, not just "wiring services together."

**Likely Follow-up Questions:**
- **Follow-up Question:** How did you handle a document that failed classification or extraction — retry, dead-letter, manual review?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should describe the actual failure-handling approach used (e.g., Service Bus dead-letter queues, retry policies, manual review queue).
- **Follow-up Question:** What Azure AI services specifically were part of this pipeline besides Document Intelligence?
  **Model Follow-up Answer:** Based on the technologies listed for this project — Azure AI Document Intelligence for extracting data from documents, Azure AI Search for retrieval, Azure OpenAI for the generative/RAG piece, and Azure Functions for orchestrating pieces of the pipeline.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Designed and implemented scalable document processing pipelines using Azure AI services for large volumes of claim-related documents."
**Why an Interviewer May Ask This:** This is an explicit design-ownership claim and will likely be probed for real architectural understanding.
**Experience Type:** Directly supported by my resume; failure-handling specifics need personal preparation.

---

### Question 9
**Question:** You mention integrating LLM-based APIs to enhance document understanding — what did that integration look like in practice?

**Model Answer:**
The idea was to use an LLM to go beyond basic extraction — things like understanding context within a document, or pulling out specific claim data points that a simpler rules-based extraction wouldn't handle well. In practice, that meant calling an LLM-based API (through Azure OpenAI) as part of the pipeline, feeding it document content or relevant extracted context, and using its output to identify and structure key claim data before it moved downstream.

**Key Points to Remember:**
- Purpose: go beyond rules-based extraction using LLM understanding.
- Integration point: LLM API call within the broader pipeline, tied to Azure OpenAI.
- Output: structured key claim data for downstream use.

**Likely Follow-up Questions:**
- **Follow-up Question:** How did you handle prompt design or make sure the LLM output was in a consistent, usable format?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't detail prompt engineering specifics; I should describe real techniques used (structured output prompting, schema validation with something like Pydantic, etc.) if applicable.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Integrated LLM-based APIs to enhance document understanding and automated extraction of key claim data."
**Why an Interviewer May Ask This:** To test practical LLM integration knowledge beyond conceptual RAG discussion.
**Experience Type:** Directly supported by my resume; implementation detail needs personal preparation.

---

### Question 10
**Question:** The resume says you worked across Python, React, Node.js, and .NET on DIP. How did these pieces fit together?

**Model Answer:**
Python was the natural fit for the AI/GenAI side — things like the document processing and LLM integration logic. .NET and Node.js were used for backend services and APIs that the AI capabilities plugged into, and React was used for the front-end that end users — like people reviewing or working with claims — would interact with. So it wasn't one single application in one language; it was more that each part of the stack was used where it made sense, and they communicated through APIs to make the AI-powered document processing accessible within the broader enterprise workflow.

**Key Points to Remember:**
- Python: AI/GenAI logic. .NET/Node.js: backend services/APIs. React: front-end.
- Integration point: APIs connecting the AI pipeline to the rest of the platform.
- This shows breadth across the stack, not just isolated AI scripting.

**Likely Follow-up Questions:**
- **Follow-up Question:** Did you personally work on all four of those, or mostly focus on one or two?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume lists all four as part of the work but doesn't break down my personal split across them; I should be ready to say honestly where I spent most of my time.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Worked across Python, React, Node.js, and .NET to integrate AI capabilities into enterprise workflows."
**Why an Interviewer May Ask This:** To understand real stack breadth versus a listed set of technologies.
**Experience Type:** Directly supported by my resume; personal split needs clarification.

---

### Question 11
**Question:** Azure AI Document Intelligence and Azure Functions are both listed as DIP technologies alongside Azure AI Search and Azure OpenAI. How did those specifically fit into the pipeline?

**Model Answer:**
Azure AI Document Intelligence was the piece handling the lower-level extraction from the raw documents — things like recognizing structure, fields, and text from forms or scanned content — before that output fed into further classification and LLM-based understanding. Azure Functions was used to orchestrate discrete steps of the pipeline — event-driven pieces triggered as documents moved through stages, especially once Service Bus was handling the messaging between those stages. So Document Intelligence handled the raw extraction layer, Azure OpenAI/RAG handled the deeper understanding layer, and Functions tied the stages together.

**Key Points to Remember:**
- Document Intelligence = raw structural/field extraction from documents.
- Azure OpenAI/RAG = deeper contextual understanding beyond structural extraction.
- Azure Functions = orchestration/glue between pipeline stages, event-driven off Service Bus.

**Likely Follow-up Questions:**
- **Follow-up Question:** Did you use pre-built models in Document Intelligence, or train/customize any models for these specific claim documents?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should confirm whether prebuilt models were sufficient or whether a custom model was trained for claim-specific document layouts.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Technologies: ...Azure AI Search, Azure AI Document Intelligence, Azure Functions, Azure Service Bus..." (DIP).
**Why an Interviewer May Ask This:** To test whether the candidate understands each named Azure service's specific role, not just that they were "used."
**Experience Type:** Directly supported by my resume; specific configuration detail needs personal preparation.

---

### Question 12
**Question:** How did the pipeline handle a document that failed classification or extraction partway through — did it just fail silently, or was there a defined failure path?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — my resume describes the pipeline as "scalable" and mentions the Service Bus migration improving "reliability," but it doesn't detail the specific failure-handling mechanism. I should be ready to describe the real approach — for example, using Service Bus dead-letter queues for messages that repeatedly failed processing, retry policies with backoff, or a manual review queue for documents that couldn't be automatically classified or extracted.

**What the interviewer is likely expecting:** A concrete failure-handling design, since "reliability" is an explicit claim tied to this project.
**What details I should prepare:** The actual retry/dead-letter/manual-review mechanism used, and how a failed document was surfaced to someone who could act on it.

**Key Points to Remember:**
- "Reliability" as a claim needs a concrete failure-handling story to back it up.
- Service Bus's dead-letter queue capability is a natural, credible mechanism to describe if accurate.

**Likely Follow-up Questions:**
- **Follow-up Question:** How would you know if a large batch of documents started failing at once?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should describe real monitoring/alerting on the pipeline, if any was in place.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Led the migration from Azure Storage Queues to Azure Service Bus, improving scalability and reliability..." combined with "designed and implemented scalable document processing pipelines."
**Why an Interviewer May Ask This:** Reliability claims invite a natural, skeptical "what happens when it fails?" follow-up.
**Experience Type:** Reasonable follow-up based on my resume; specific mechanism needs personal preparation.

---

### C. Healthcare Staffing Platform (Neudesic, Jul 2022–Jan 2025)

---

### Question 13
**Question:** Tell me about the healthcare staffing platform project and your role on the Notifications Team.

**Model Answer:**
This was a platform connecting healthcare facilities with nurses and healthcare professionals — so think of it as a marketplace/staffing type of application. I was part of the Notifications Team, which was responsible for reliable multi-channel communication — making sure the right notifications reached the right people through the right channels. I worked on both the front-end, building features in React, TypeScript, and Redux-Saga, and the back-end, building services in .NET and C# that supported real-time notification workflows.

**Key Points to Remember:**
- Domain: healthcare staffing marketplace connecting facilities and healthcare professionals.
- Team: Notifications Team — multi-channel communication focus.
- Full-stack involvement: React/TypeScript/Redux-Saga (frontend) + .NET/C# (backend).

**Likely Follow-up Questions:**
- **Follow-up Question:** What does "multi-channel" mean here — email, SMS, push notifications?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't list the specific channels; I should recall the actual notification channels the platform supported.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Worked on a healthcare staffing platform connecting healthcare facilities with nurses and healthcare professionals, contributing to the Notifications Team and reliable multi-channel communication workflows."
**Why an Interviewer May Ask This:** To establish context on the earliest, longest-running project and pre-GenAI experience.
**Experience Type:** Directly supported by my resume.

---

### Question 14
**Question:** How did you use Redux-Saga specifically, and why was it needed over plain Redux?

**Model Answer:**
Redux-Saga was used to manage side effects in a more structured way than plain Redux thunks would allow — things like handling asynchronous API calls, sequencing notification-related actions, or coordinating more complex flows tied to real-time updates. In a real-time notification context, you often need to manage multiple asynchronous events cleanly, and sagas gave us a way to express that logic as testable, manageable flows rather than scattering async logic across components or reducers.

**Key Points to Remember:**
- Redux-Saga's role: managing side effects/async flows more explicitly than thunks.
- Real-time notification context benefits from structured async flow handling.
- This is conceptual/general knowledge applied to the resume's real-time notification context.

**Likely Follow-up Questions:**
- **Follow-up Question:** Can you describe a specific saga you built and what flow it managed?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't detail a specific saga; I should recall a real feature I implemented with Redux-Saga.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Built scalable frontend features using React, TypeScript, and Redux-Saga."
**Why an Interviewer May Ask This:** Redux-Saga is a specific, deliberate technology choice worth probing for real understanding.
**Experience Type:** Conceptual knowledge connected to my resume; specific example needs personal preparation.

---

### Question 15
**Question:** What did "real-time notification workflows" on the backend actually involve technically?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume states I "developed backend services using .NET and C# supporting real-time notification workflows," but it doesn't specify the exact mechanism (e.g., SignalR, polling, queue-based triggers, webhooks). I should prepare a clear, accurate description of the actual technical approach used to deliver notifications in real time, since this is a natural and likely follow-up.

**What the interviewer is likely expecting:** A concrete technical mechanism — e.g., how a notification event was triggered, processed, and delivered to a user in near real time.
**What details I should prepare:** The actual delivery mechanism used (SignalR, message queue, polling, third-party notification service), how failures/retries were handled, and one concrete example of a notification workflow I built.

**Key Points to Remember:**
- Don't guess a mechanism that isn't accurate — recall the real implementation.
- Be ready to explain why that mechanism was chosen over alternatives.
- Tie it back to "system reliability," which the resume explicitly calls out as a focus area.

**Likely Follow-up Questions:**
- **Follow-up Question:** How did you ensure reliability — e.g., what happens if a notification fails to send?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — describe real retry/failure-handling logic used.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Developed backend services using .NET and C# supporting real-time notification workflows."
**Why an Interviewer May Ask This:** To verify genuine backend/real-time systems experience beyond a generic resume line.
**Experience Type:** Directly supported by my resume for the "what"; mechanism detail is a preparation gap.

---

### Question 16
**Question:** Why did the platform need Azure AI Search for communication logs specifically?

**Model Answer:**
Communication logs can grow very large and unstructured — lots of messages and notifications over time — so simple database filtering isn't always efficient or flexible enough when people need to search across that history. Azure AI Search gave us better search capabilities across those logs, making it easier to find relevant communication history quickly rather than relying on basic queries against a relational store.

**Key Points to Remember:**
- Problem: large volume of unstructured/semi-structured communication log data.
- Azure AI Search's role: improved search/retrieval over that data.
- This predates the GenAI-focused roles — shows earlier, non-GenAI use of the same technology.

**Likely Follow-up Questions:**
- **Follow-up Question:** Was this a full-text search implementation, or did it involve any semantic/vector search?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't specify; given the era of this project (2022–2025) relative to the later GenAI-focused roles, I should clarify whether this was primarily keyword/full-text search or included semantic capabilities.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Integrated Azure AI Search to enhance search capabilities across communication logs."
**Why an Interviewer May Ask This:** Azure AI Search appears across multiple projects — interviewer will want to see how usage evolved over time.
**Experience Type:** Directly supported by my resume; implementation depth needs clarification.

---

### D. CDK Global — Internship (Aug 2021–Jun 2022)

---

### Question 17
**Question:** Tell me about your internship at CDK Global — what did you work on, and what led to the full-time offer?

**Model Answer:**
During my internship, I developed full-stack enterprise features using React, .NET 6, C#, and PostgreSQL — building both APIs and UI components that supported enterprise workflows. I also worked with tools like SonarQube and Splunk to help maintain application quality. Based on how that internship went, I was offered a full-time position, which is really where my professional software engineering career started.

**Key Points to Remember:**
- Full-stack scope even as an intern: React frontend + .NET 6/C# backend + PostgreSQL.
- Quality tooling exposure early on: SonarQube, Splunk.
- Outcome: full-time offer — good entry point for a career-progression/behavioral question.

**Likely Follow-up Questions:**
- **Follow-up Question:** What's a specific feature you built during the internship that you're proud of?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should recall one concrete feature to describe with specifics.
- **Follow-up Question:** What did SonarQube and Splunk help you catch or understand?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should recall a real example: a code quality issue SonarQube flagged, or something I diagnosed using Splunk logs.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Developed full-stack enterprise features using React, .NET 6, C#, and PostgreSQL... Received a full-time offer based on internship performance."
**Why an Interviewer May Ask This:** Standard entry point into a candidate's career narrative and growth trajectory.
**Experience Type:** Directly supported by my resume; specific feature examples need personal preparation.

---

## Part 2: Skills-Based Questions

### Strong Resume Evidence

---

### Question 18
**Question:** Azure OpenAI and RAG appear across multiple roles — GreenShield and DIP. How has your use of them evolved or differed between those two projects?

**Model Answer:**
Both projects used Azure OpenAI and RAG for a similar underlying purpose — processing unstructured insurance-related documents and extracting meaningful information. At DIP, I was more involved in the surrounding pipeline design and infrastructure, like the messaging migration and the overall scalable processing pipeline, with GenAI as one part of a bigger system. At GreenShield, my focus was more directly on developing and integrating the GenAI capabilities themselves into the claims workflow. So the core technology usage is similar, but the scope of my involvement — pipeline/infrastructure versus direct capability integration — was different.

**Key Points to Remember:**
- Same core tech (Azure OpenAI, RAG) but different scope of personal involvement between the two projects.
- DIP: pipeline/infra-heavy. GreenShield: capability integration-focused.
- Both are insurance-domain, unstructured document processing use cases.

**Likely Follow-up Questions:**
- **Follow-up Question:** Which project gave you deeper hands-on RAG implementation experience?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should decide honestly which project had more of my direct hands-on RAG work and be consistent about it.

**Resume Reference and Interview Context:**
**Resume Trigger:** Azure OpenAI and RAG appear in both the DIP and GreenShield technology lists.
**Why an Interviewer May Ask This:** To assess depth and consistency of GenAI experience across roles rather than a single one-off exposure.
**Experience Type:** Directly supported by my resume.

---

### Question 19
**Question:** You led a migration to Azure Service Bus at DIP. What specific Service Bus features and capabilities have you actually worked with?

**Model Answer:**
At DIP, leading that migration meant getting hands-on with Service Bus beyond just the basic queue concept — figuring out which capabilities we needed, like topics and subscriptions for routing messages to different business-unit consumers, to support that parallel processing requirement. It's a step up from Storage Queues in terms of what you can actually configure and rely on for a higher-throughput, multi-consumer scenario like that document processing pipeline.

**Key Points to Remember:**
- This is currently my only hands-on Service Bus project, so I should know it in depth rather than broadly.
- Emphasize the specific features tied to the actual business need: parallel processing across business units.

**Likely Follow-up Questions:**
- **Follow-up Question:** What Service Bus features have you actually used — queues, topics/subscriptions, sessions, dead-lettering?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should be specific about which Service Bus features I've actually configured or used, rather than speaking generically.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Led the migration from Azure Storage Queues to Azure Service Bus, improving scalability and reliability while enabling parallel processing across multiple business units." (DIP)
**Why an Interviewer May Ask This:** Repeated emphasis on Service Bus in the resume signals real depth — interviewer will test whether that depth is genuine.
**Experience Type:** Directly supported by my resume.

---

### Question 20
**Question:** Azure AI Search appears on the Healthcare Staffing project (2022–2025) and the GenAI projects (2025–present). How has your understanding of it changed over that time?

**Model Answer:**
On the healthcare staffing platform, I used Azure AI Search mainly to improve search over communication logs — more of a traditional search-over-data-store use case. In the later GenAI-focused projects, Azure AI Search plays a role connected to RAG — being the retrieval component that feeds relevant content into a GenAI pipeline. So the underlying service is the same, but my use of it shifted from a standalone search improvement to being a building block within a retrieval-augmented generation pipeline.

**Key Points to Remember:**
- Early use: standalone search over communication logs.
- Later use: retrieval component within RAG-based GenAI pipelines.
- Shows a believable progression from traditional search to AI-era usage.

**Likely Follow-up Questions:**
- **Follow-up Question:** Did the earlier project use any vector or semantic search capabilities of Azure AI Search, or just keyword search?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should clarify honestly, since semantic/vector search maturity in Azure AI Search and my exposure to it likely differed at that earlier point in my career.

**Resume Reference and Interview Context:**
**Resume Trigger:** Azure AI Search listed in Healthcare Staffing, DIP, and GreenShield technology lists.
**Why an Interviewer May Ask This:** To trace skill progression over a multi-year career and validate a coherent growth story.
**Experience Type:** Directly supported by my resume.

---

### Question 21
**Question:** React and .NET/C# appear across almost every role on your resume — from the CDK internship through to Healthcare Staffing and DIP. How would you describe your depth with these compared to your newer GenAI/Python skills?

**Model Answer:**
React, .NET, and C# are where I have the longest and most consistent hands-on experience — going all the way back to my internship and carrying through the healthcare staffing platform and into DIP. My Python and GenAI experience is more recent, about two years, layered on top of that existing software engineering foundation. So I'd describe it as: strong, long-standing depth in React/.NET/C# for building enterprise applications, with GenAI/Python being where I've focused my growth over the last couple of years, often integrating with the same kind of .NET/React-based systems I already knew well.

**Key Points to Remember:**
- React/.NET/C# = longest, most consistent experience (2021–present).
- Python/GenAI = newer, ~2 years, but built on top of solid engineering fundamentals.
- Frame GenAI skills as an extension of existing engineering strength, not a replacement for it.

**Likely Follow-up Questions:**
- **Follow-up Question:** Would you consider yourself stronger in backend (.NET/C#) or frontend (React) work?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should give an honest, specific answer based on where I've spent more time and felt more effective.

**Resume Reference and Interview Context:**
**Resume Trigger:** React, .NET, C# appear in CDK Global, Healthcare Staffing, and DIP experience sections and Core Skills.
**Why an Interviewer May Ask This:** To understand the balance between long-standing engineering skills and newer AI specialization.
**Experience Type:** Directly supported by my resume.

---

### Question 22
**Question:** Python is central to your GenAI work — what's your comfort level with it compared to your original C#/.NET background?

**Model Answer:**
Python became central to my work once I moved into Generative AI, since it's the natural ecosystem for that — things like calling LLM APIs, working with RAG frameworks, and general AI service integration. My original background is C# and .NET, so Python was something I built up specifically for the GenAI-focused roles over the last roughly two years. I'm comfortable writing production-level Python for these AI integration and document-processing pipelines, though my longest-standing depth is still on the .NET/C# side.

**Key Points to Remember:**
- Python skill is GenAI-driven, roughly 2 years, layered on a C#/.NET foundation.
- Be honest about relative depth rather than claiming equal expert-level in both.
- Ties back to the resume's "2+ years of hands-on experience in Generative AI."

**Likely Follow-up Questions:**
- **Follow-up Question:** What Python libraries or frameworks have you actually used in these projects — LangChain, LangGraph, requests/HTTP clients, Pydantic?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — LangChain, LangGraph, and Pydantic are listed as skills; I should be ready to say specifically which ones I've actually used hands-on versus which are more exposure-level.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Python" listed centrally across GreenShield, DIP, and the Generative AI & AI Engineering skills section.
**Why an Interviewer May Ask This:** To calibrate expectations for Python-specific technical questions later in the interview.
**Experience Type:** Directly supported by my resume.

---

### Listed as a Skill (Limited Explicit Project Evidence)

---

### Question 23
**Question:** Your skills list LangChain, LangGraph, AI Agents, and Function Calling. Can you tell me about your hands-on experience with these specifically?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — these appear in my Core Skills section, but my project descriptions for GreenShield and DIP talk about RAG, Azure OpenAI, and LLM-based APIs in general terms rather than explicitly naming LangChain, LangGraph, agent frameworks, or function calling. I need to be ready to say clearly and honestly which of these I've used hands-on in a real project (and in which one), versus which I know conceptually or have used in smaller/learning contexts, since an interviewer may ask me to point to a specific example.

**What the interviewer is likely expecting:** A specific example — e.g., "I used LangChain for X in project Y" — or an honest admission of conceptual-only familiarity.
**What details I should prepare:** Whether I actually used LangChain/LangGraph within DIP or GreenShield's LLM integration work, or in a personal/learning project; a concrete example of function calling or agent behavior I've implemented, if any.

**Key Points to Remember:**
- Don't claim project-level experience with a framework unless it's true and specific.
- It's fine to say "conceptual understanding, applied at a smaller scale" if that's accurate.
- Certifications (IBM Generative & Agentic AI Developer/Foundation) can support conceptual credibility here.

**Likely Follow-up Questions:**
- **Follow-up Question:** Can you explain how function calling works conceptually, even if you haven't built a full agent system?
  **Model Follow-up Answer:** Function calling lets an LLM decide, based on the prompt and context, to invoke a specific defined function/tool with structured parameters rather than just returning free text — the model output is used to trigger real code execution, which is what allows LLMs to take actions or fetch live data rather than only generating text.

**Resume Reference and Interview Context:**
**Resume Trigger:** "AI Agents, Function Calling... LangChain, LangGraph" listed in Core Skills — Generative AI & AI Engineering.
**Why an Interviewer May Ask This:** These are more advanced, specific technologies listed without a clear project-level anchor in the experience section.
**Experience Type:** Listed as a skill — requires honest clarification of depth.

---

### Question 24
**Question:** Embeddings and Vector Search are on your skills list — where have you actually applied these concepts?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — Vector Search and Embeddings are listed as skills, and they're conceptually central to how RAG works, which I did apply in GreenShield and DIP. But my resume's project bullets describe RAG and Azure AI Search at a workflow level rather than explicitly detailing embedding generation or vector search configuration. I should be ready to explain whether I directly worked with generating/storing embeddings and configuring vector search (e.g., in Azure AI Search's vector capabilities), or whether that layer was mostly handled by the platform/service and my work was more at the RAG pipeline/integration level.

**What the interviewer is likely expecting:** Understanding of how embeddings and vector search technically support RAG, and clarity on whether I configured this directly or consumed it through a managed service.
**What details I should prepare:** Whether I set up vector indexes in Azure AI Search myself, chose an embedding model, or primarily worked at the pipeline/orchestration level above that.

**Key Points to Remember:**
- These are the technical foundation of RAG — conceptually I should be fluent regardless.
- Be honest about whether I configured this layer directly or used it through a managed Azure service.

**Likely Follow-up Questions:**
- **Follow-up Question:** How does vector search differ from traditional keyword search?
  **Model Follow-up Answer:** Vector search compares the semantic meaning of a query against document embeddings using similarity (like cosine similarity) rather than matching exact keywords, which lets it find conceptually relevant content even when the wording is different — that's what makes it useful for grounding LLM responses in RAG.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Embeddings, Vector Search" listed under Generative AI & AI Engineering skills.
**Why an Interviewer May Ask This:** To test whether RAG usage is backed by real understanding of its technical foundations.
**Experience Type:** Conceptual knowledge connected to my resume; hands-on depth needs clarification.

---

### Question 25
**Question:** Docker is listed in your DevOps & Tools skills, but I don't see it explicitly mentioned in any project bullet. Where have you used it?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — Docker is listed under my DevOps & Tools skills, but none of my project bullets explicitly call out containerization work. I should be ready to explain honestly where I've actually used Docker — whether that's containerizing a service in one of these projects, using it for local development environments, or a more limited/learning-level exposure — rather than implying a level of production container orchestration experience I can't back up.

**What the interviewer is likely expecting:** A specific, honest answer about real Docker usage — even a modest one, like using it for local dev setup — is fine; vague or evasive answers are a bigger risk.
**What details I should prepare:** A concrete example: which service I containerized, why, and what the Dockerfile/setup looked like, if applicable.

**Key Points to Remember:**
- This is a "listed skill" gap — be prepared to answer honestly and specifically.
- A modest, honest answer is better than implying deep production container/orchestration experience not shown elsewhere on the resume.

**Likely Follow-up Questions:**
- **Follow-up Question:** Have you worked with container orchestration like Kubernetes or Azure Container Apps?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — neither is listed on my resume; I should answer honestly based on real exposure, if any, rather than overstating it.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Docker" listed under DevOps & Tools in Core Skills.
**Why an Interviewer May Ask This:** No project bullet supports this skill directly — a natural gap to probe.
**Experience Type:** Listed as a skill — requires honest clarification.

---

### Question 26
**Question:** LLM Evaluation is listed as a skill. How do you approach evaluating whether an LLM-based feature is actually working well?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — this is listed in my Core Skills, but my project bullets for GreenShield and DIP don't explicitly describe an evaluation process. I should prepare a concrete example of how I actually assessed the quality of LLM output in one of these projects — for example, comparing extracted data against ground truth, manual review sampling, or tracking specific accuracy/error metrics — rather than only giving a textbook definition of LLM evaluation.

**What the interviewer is likely expecting:** A real, applied example tied to GreenShield or DIP, showing how extraction or generation quality was actually verified.
**What details I should prepare:** What "good" output looked like in these projects, how deviations were caught, and whether any structured evaluation process or metrics were used.

**Key Points to Remember:**
- Conceptually, LLM evaluation involves accuracy, relevance, consistency, and sometimes automated or human-in-the-loop scoring.
- I should tie this back to a real project rather than answering only in the abstract.

**Likely Follow-up Questions:**
- **Follow-up Question:** What conceptual approaches to LLM evaluation are you aware of, even outside your own project experience?
  **Model Follow-up Answer:** Common approaches include human review/labeling of a sample of outputs, comparing against a ground-truth or reference answer set, and using metrics or even another LLM as a judge to score things like relevance, factual accuracy, and completeness.

**Resume Reference and Interview Context:**
**Resume Trigger:** "LLM Evaluation" listed under Generative AI & AI Engineering skills.
**Why an Interviewer May Ask This:** No explicit evaluation process is described in the project bullets — a natural gap to probe.
**Experience Type:** Listed as a skill — requires honest, applied clarification.

---

## Part 3: Claim Validation Questions

---

### Question 27
**Question:** Your summary says you have "proven ability to design and deliver scalable, production-ready AI applications." Which project best demonstrates that, and what specifically did you design?

**Model Answer:**
The strongest example is DIP, the Document Intelligence Platform at Neudesic — that's where I designed and implemented the scalable document processing pipeline using Azure AI services, and led the migration to Azure Service Bus to support scalability and parallel processing across business units. That combination — pipeline design plus the messaging infrastructure change — is what I'd point to as actual design and delivery work, rather than just integrating a pre-built capability.

**Key Points to Remember:**
- Best evidence: DIP — the only project with explicit "designed and implemented" and "led" language.
- Avoid pointing to GreenShield as a "design" example — that's described as "developed/integrated," not designed.
- Be ready to describe the pipeline design in technical detail (see Question 8).

**Likely Follow-up Questions:**
- **Follow-up Question:** Was this pipeline actually running in production, and do you know how it performed after launch?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should confirm and describe the production status and any real post-launch outcomes I observed.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Proven ability to design and deliver scalable, production-ready AI applications by combining Generative AI with robust software engineering and Azure cloud architecture."
**Why an Interviewer May Ask This:** This is the boldest claim in the professional summary — interviewers typically ask candidates to substantiate summary-level claims with a concrete example.
**Experience Type:** Directly supported by my resume (via DIP); requires confident, specific delivery.

---

### Question 28
**Question:** I notice the GreenShield project (Dec 2025–Jun 2026) is listed under your IBM role, but your IBM title/role start date is Feb 2026, and your Neudesic role runs through Jan 2026. Can you clarify that timeline?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — this is a genuine, minor overlap in how the dates are laid out on the resume that I should be ready to clarify plainly and honestly, rather than getting caught off guard. It's likely related to Neudesic being an IBM company and a transition period between the two roles, but I should confirm the accurate real-world timeline (e.g., whether GreenShield actually started while I was still formally at Neudesic before transitioning to the IBM title) so I can explain it clearly and consistently if asked.

**What the interviewer is likely expecting:** A clear, confident, non-defensive clarification of the actual employment/role timeline.
**What details I should prepare:** The exact real dates of transition from Neudesic to IBM, and whether GreenShield began under Neudesic and continued after the move to IBM.

**Key Points to Remember:**
- This is a resume-detail question, not a technical one — answer it factually and move on.
- Don't be defensive; a clean, brief clarification is enough.

**Likely Follow-up Questions:**
- **Follow-up Question:** So is IBM the same overall employer as Neudesic, just after an internal transition?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should confirm the actual relationship (e.g., Neudesic was acquired by / operates as part of IBM) and describe it accurately.

**Resume Reference and Interview Context:**
**Resume Trigger:** GreenShield (Dec 2025–Jun 2026) listed under "IBM — India, Senior Application Consultant, Feb 2026 – Present," while Neudesic role runs "Jan 2025 – Jan 2026."
**Why an Interviewer May Ask This:** A careful interviewer reviewing dates closely may notice the slight inconsistency and ask for clarification.
**Experience Type:** Reasonable follow-up based on my resume.

---

### Question 29
**Question:** You moved from Intern → Consultant → Senior Consultant → Senior Application Consultant in a relatively short span, and Neudesic became "an IBM Company" along the way. Can you walk me through that progression?

**Model Answer:**
I started as an intern at CDK Global, which turned into a full-time offer, and that's really where my professional career began. I then joined Neudesic as a Consultant in mid-2022, working on the healthcare staffing platform for a couple of years, and was promoted to Senior Consultant in early 2025, which is when I moved onto the DIP project and shifted more into Generative AI work. Neudesic operates as an IBM company, and I then moved into a Senior Application Consultant role at IBM in 2026, continuing to work on Generative AI projects like GreenShield.

**Key Points to Remember:**
- Clear chronological narrative: CDK intern → Neudesic Consultant → Neudesic Senior Consultant → IBM Senior Application Consultant.
- Frame the Neudesic-to-IBM move as a natural continuation (Neudesic is "an IBM Company"), not a job change with a gap in continuity.
- Tie career growth to increasing responsibility and a deliberate shift toward GenAI starting around the DIP project.

**Likely Follow-up Questions:**
- **Follow-up Question:** What changed in your day-to-day responsibilities when you became "Senior" at Neudesic?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — the resume doesn't detail this explicitly; I should describe real changes (e.g., more design/leadership involvement, as reflected in the DIP project's "designed" and "led" language).
- **Follow-up Question:** What motivated your shift toward Generative AI specifically around 2025?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should give a genuine, personal reason (interest in the technology, project opportunity, market direction, etc.).

**Resume Reference and Interview Context:**
**Resume Trigger:** Full employment history across CDK Global, Neudesic (an IBM Company), and IBM, with escalating titles.
**Why an Interviewer May Ask This:** To understand career motivation, growth trajectory, and consistency of the narrative.
**Experience Type:** Directly supported by my resume; motivational detail needs personal preparation.

---

### Question 30
**Question:** GreenShield is your current, live GenAI project. What makes you confident it's actually production-grade rather than a proof of concept?

**Model Answer:**
`[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — my resume describes GreenShield as developing and integrating GenAI capabilities "into enterprise insurance workflows," which implies real, in-use functionality rather than a pure prototype, but it doesn't explicitly state production deployment status. I should be ready to confirm honestly whether the work I did has shipped to production, is in a pilot/rollout phase, or is still being validated, and describe what "done" looked like for the pieces I personally built.

**What the interviewer is likely expecting:** A clear, honest statement of where the project actually stands (live/production vs. pilot), not an assumption that everything on a resume is fully shipped.
**What details I should prepare:** The actual deployment/rollout status of GreenShield and what my specific deliverables looked like when I considered them complete.

**Key Points to Remember:**
- Don't overstate production status if the reality is closer to pilot/rollout.
- Being precise here builds credibility for every other claim on the resume.

**Likely Follow-up Questions:**
- **Follow-up Question:** How is GreenShield being used day-to-day by the business team right now?
  **Model Follow-up Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should describe the actual current usage pattern.

**Resume Reference and Interview Context:**
**Resume Trigger:** "Integrated GenAI capabilities into enterprise insurance workflows in collaboration with engineering and business teams." (GreenShield)
**Why an Interviewer May Ask This:** GreenShield is the most recent, currently active GenAI project — a natural place to verify real-world status versus resume phrasing.
**Experience Type:** Reasonable follow-up based on my resume; status detail needs personal preparation.

---

## Part 4: Interview Follow-up Chains (Deep Drill-Downs)

### Chain 1 — DIP: Leading the Storage Queues → Service Bus Migration

**Interviewer Question:** You say you led the migration from Azure Storage Queues to Azure Service Bus on the Document Intelligence Platform. What triggered that decision?

**Candidate Model Answer:** As the platform needed to support parallel document processing across multiple business units, Storage Queues started to show its limitations — it's a simpler queuing mechanism without some of the more advanced messaging patterns needed for that kind of multi-consumer, parallel workload. Service Bus offered better support for that, so I led the effort to migrate the messaging layer to it.

**Interviewer Follow-up 1:** What specific Service Bus features did you rely on to enable that parallel processing across business units — topics/subscriptions, sessions, something else?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should recall precisely which Service Bus feature (e.g., topics and subscriptions to let each business unit's consumer process relevant messages independently) was the key enabler here, since this is a natural technical follow-up to my "led the migration" claim.

**Interviewer Follow-up 2:** How did you make sure no messages were lost or duplicated during the cutover from Storage Queues to Service Bus?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I need a concrete answer here: whether there was a dual-write/parallel-run period, how in-flight messages in the old queue were drained before cutover, and how idempotency or deduplication was handled if at all.

**Interviewer Follow-up 3:** Looking back, is there anything about that migration you'd do differently?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — this is a genuine reflection question; I should have a real, honest answer (e.g., something about monitoring, phased rollout, or documentation) rather than a generic "no, it went perfectly" response, which would sound unconvincing.

---

### Chain 2 — RAG for Insurance Document Processing (GreenShield / DIP)

**Interviewer Question:** Both GreenShield and DIP involve RAG for processing insurance documents. Can you explain how RAG was applied there?

**Candidate Model Answer:** In both cases, the goal was to ground GenAI output in the actual content of insurance documents rather than relying only on a model's general knowledge — so relevant document content would be retrieved, using Azure AI Search, and provided as context to the model before it generated a response or extracted structured data. This made the extraction more accurate and grounded in the source document, which matters in an insurance claims context.

**Interviewer Follow-up 1:** How was the document content chunked and indexed for retrieval?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should describe the actual chunking strategy used (e.g., by section, page, or fixed token size) and how it was indexed in Azure AI Search, since this is a fundamental RAG implementation detail an interviewer is likely to probe.

**Interviewer Follow-up 2:** What happens if the retrieval step pulls in irrelevant or incorrect context — how does that affect the final output, and how would you mitigate it?

**Candidate Model Answer:** If retrieval pulls in irrelevant context, the model's output can become inaccurate or reference the wrong part of a document, so mitigating it usually involves improving retrieval quality — like better chunking, filtering, or re-ranking retrieved results — as well as designing prompts that instruct the model to rely strictly on the provided context and indicate when the context doesn't contain a clear answer.

**Interviewer Follow-up 3:** How confident are you that this approach would scale to a much larger and more diverse set of insurance document types than what you worked with?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should reflect honestly on the actual diversity/scale of documents I worked with and speak to real, known limitations rather than overpromising scalability I haven't validated myself.

---

### Chain 3 — GreenShield: Turning Business Requirements into an AI Workflow

**Interviewer Question:** Tell me about GreenShield and how business requirements shaped the GenAI workflow you built.

**Candidate Model Answer:** GreenShield was about developing and integrating GenAI capabilities for insurance claims processing, and that meant working closely with both engineering and business teams — understanding what data points the business actually needed extracted from claims documents and what counted as accurate, usable output, then building the Azure OpenAI/RAG-based workflow around those needs.

**Interviewer Follow-up 1:** What happens when the business team says the extraction "isn't accurate enough" — how do you even quantify that?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I need a real answer about how accuracy expectations were defined and measured with the business team, since "accurate enough" is subjective without a concrete process behind it.

**Interviewer Follow-up 2:** Did the business requirements ever conflict with what was technically feasible with the GenAI approach you were using?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should recall a real instance of a requirement that pushed against a technical limitation (e.g., document quality, model context length, or extraction consistency) and how that was resolved or compromised on.

**Interviewer Follow-up 3:** Looking back, is there a business requirement you'd push back on differently today?

**Candidate Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — a genuine reflection question; I should have an honest, specific answer rather than claiming everything went smoothly.

---

## Part 5: Question Prioritization

### Tier 1 — Must Prepare
Most strongly connected to current GenAI projects, flagship project, and headline claims.
- Q1, Q2 — GreenShield overview and RAG usage (current project)
- Q6, Q7, Q8, Q9 — DIP (strongest GenAI project, explicit design/leadership claims)
- Q27 — "design and deliver scalable, production-ready AI applications" summary claim
- Chain 1, Chain 2 — deep drill-downs on DIP's Service Bus migration and RAG usage

### Tier 2 — Very Likely
Significant prior projects, deeper GenAI detail, and well-evidenced skills.
- Q3, Q4, Q5 — GreenShield deeper technical and collaboration detail
- Q10, Q11, Q12 — DIP cross-stack integration, Document Intelligence/Functions specifics, failure handling
- Q13, Q14, Q15, Q16 — Healthcare Staffing Platform
- Q18, Q19, Q20, Q21, Q22 — Strong-evidence skills (Azure OpenAI/RAG, Service Bus, Azure AI Search, React/.NET, Python)
- Chain 3 — GreenShield business/engineering collaboration deep dive
- Q29 — Career progression narrative

### Tier 3 — Possible Follow-ups
May arise if interviewer explores further or is more skeptical.
- Q17 — CDK Global internship
- Q23, Q24, Q25, Q26 — Skills listed without strong project evidence (LangChain/LangGraph, Vector Search/Embeddings, Docker, LLM Evaluation)
- Q28 — Resume date/timeline clarification
- Q30 — GreenShield production-readiness clarification

---

## Part 6: Areas Requiring More Preparation

### Area 1
**Resume Statement:** "Led the migration from Azure Storage Queues to Azure Service Bus... enabling parallel processing across multiple business units." (DIP)
**Potential Interview Risk:** This is the strongest ownership claim on the resume; interviewer will drill into cutover mechanics, specific Service Bus features used, and any measurable outcome.
**Details I Should Prepare:** Cutover approach, message-loss prevention, specific Service Bus features (topics/subscriptions, sessions), and any real before/after observation (without inventing numbers).

### Area 2
**Resume Statement:** "Designed and implemented scalable document processing pipelines using Azure AI services." (DIP)
**Potential Interview Risk:** Interviewer may ask for architecture-level detail (failure handling, retries, monitoring) beyond the resume's summary phrasing.
**Details I Should Prepare:** Concrete pipeline stages, failure/retry handling, and specific Azure AI services orchestrated together (Document Intelligence, Azure Functions, Azure OpenAI, Azure AI Search).

### Area 3
**Resume Statement:** GreenShield — "Developed and integrated Generative AI capabilities for insurance claims processing and document understanding."
**Potential Interview Risk:** Interviewer may want specifics on document types, model/prompt choices, and how output quality was verified.
**Details I Should Prepare:** Real document types handled, the specific Azure OpenAI model/prompting approach, a specific extraction scenario, and how correctness was checked.

### Area 4
**Resume Statement:** GreenShield — "Integrated GenAI capabilities into enterprise insurance workflows in collaboration with engineering and business teams."
**Potential Interview Risk:** Interviewer may probe how business requirements actually shaped the technical build, and whether the project is genuinely in production use.
**Details I Should Prepare:** A concrete example of a business requirement that shaped the AI workflow, and the actual deployment/rollout status of the project.

### Area 5
**Resume Statement:** "Developed backend services using .NET and C# supporting real-time notification workflows" (Healthcare Staffing).
**Potential Interview Risk:** Resume doesn't specify the real-time delivery mechanism (SignalR, polling, queue-based, third-party service).
**Details I Should Prepare:** The actual technical mechanism used for real-time delivery and how reliability/failure handling was addressed.

### Area 6
**Resume Statement:** GreenShield project dates (Dec 2025–Jun 2026) listed under the IBM role, which itself starts Feb 2026, while the Neudesic role runs through Jan 2026.
**Potential Interview Risk:** A detail-oriented interviewer may notice the overlap and ask for clarification.
**Details I Should Prepare:** The precise real transition timeline between Neudesic and IBM, and when GreenShield actually started relative to that.

### Area 7
**Resume Statement:** Skills listed without explicit project narrative — LangChain, LangGraph, AI Agents, Function Calling, LLM Evaluation, Vector Search, Embeddings, Pydantic, Docker.
**Potential Interview Risk:** Interviewer may ask "where did you use this?" and a vague answer will hurt credibility more than an honest "conceptual understanding" answer.
**Details I Should Prepare:** For each, decide honestly: hands-on project use (which project, specifically — GreenShield or DIP) vs. conceptual/learning-level familiarity.

### Area 8
**Resume Statement:** CDK Global internship — "Received a full-time offer based on internship performance."
**Potential Interview Risk:** A natural behavioral question about what made the internship successful; a vague answer misses an easy opportunity to show growth.
**Details I Should Prepare:** A specific feature or contribution during the internship and honest reflection on what I learned.

---

## Final Output

### 1. Top 30 Most Likely Interview Questions (Prioritized Index)

The full Question → Model Answer → Key Points → Follow-ups → Resume Reference content for each of these is in Parts 1–3 above. This index lists them in priority order for quick revision.

1. Q1 — GreenShield project overview *(Tier 1)*
2. Q2 — RAG usage in GreenShield *(Tier 1)*
3. Q6 — DIP platform overview *(Tier 1)*
4. Q7 — Leading Storage Queues → Service Bus migration *(Tier 1)*
5. Q8 — Designing scalable document processing pipelines *(Tier 1)*
6. Q9 — Integrating LLM-based APIs for document understanding *(Tier 1)*
7. Q27 — Evidence behind "design and deliver scalable, production-ready AI applications" *(Tier 1)*
8. Q3 — Challenges extracting from unstructured insurance documents *(Tier 2)*
9. Q4 — Azure OpenAI model/prompting approach in GreenShield *(Tier 2)*
10. Q5 — Collaboration with business teams on GreenShield *(Tier 2)*
11. Q10 — Python/React/Node.js/.NET integration on DIP *(Tier 2)*
12. Q11 — Azure AI Document Intelligence and Azure Functions specifics in DIP *(Tier 2)*
13. Q12 — Failure handling in the DIP pipeline *(Tier 2)*
14. Q13 — Healthcare staffing platform and Notifications Team role *(Tier 2)*
15. Q14 — Redux-Saga usage and rationale *(Tier 2)*
16. Q15 — Real-time notification workflow mechanics *(Tier 2)*
17. Q16 — Why Azure AI Search for communication logs *(Tier 2)*
18. Q18 — Azure OpenAI/RAG usage compared across DIP and GreenShield *(Tier 2)*
19. Q19 — Azure Service Bus feature depth at DIP *(Tier 2)*
20. Q20 — Azure AI Search evolution across career *(Tier 2)*
21. Q21 — React/.NET depth vs. newer GenAI/Python skills *(Tier 2)*
22. Q22 — Python comfort level vs. C#/.NET background *(Tier 2)*
23. Q29 — Career progression narrative (CDK → Neudesic → IBM) *(Tier 2)*
24. Q17 — CDK Global internship and full-time offer *(Tier 3)*
25. Q23 — LangChain/LangGraph/AI Agents/Function Calling depth *(Tier 3)*
26. Q24 — Embeddings/Vector Search hands-on depth *(Tier 3)*
27. Q25 — Docker hands-on depth *(Tier 3)*
28. Q26 — LLM evaluation approach *(Tier 3)*
29. Q28 — GreenShield/IBM timeline clarification *(Tier 3)*
30. Q30 — GreenShield production-readiness clarification *(Tier 3)*

### 2. Claims I Must Be Ready to Explain

- **"Proven ability to design and deliver scalable, production-ready AI applications."** I should be able to explain, in technical detail, the DIP pipeline design and the Storage Queues → Service Bus migration — the only project where "designed"/"led" language is explicitly used. I must be clear that this is the evidence for the claim, not GreenShield.
- **"Led the migration from Azure Storage Queues to Azure Service Bus... enabling parallel processing across multiple business units."** I should know the specific Service Bus features used, the cutover approach, and be ready to discuss (without inventing numbers) the real impact.
- **"Designed and implemented scalable document processing pipelines using Azure AI services."** I should know the pipeline stages, failure handling, and which specific Azure AI services were orchestrated together.
- **"Developed and integrated Generative AI capabilities" (GreenShield).** I should have a concrete example of a document type, model/prompt approach, and extraction scenario ready, since the resume bullet is somewhat general.
- **"Integrated GenAI capabilities into enterprise insurance workflows in collaboration with engineering and business teams" (GreenShield).** I should have a specific example of a business requirement that shaped the AI workflow, and clarity on the project's real deployment status.
- **Cloud-native applications, microservices, distributed systems (summary claim).** I should be ready to connect this general claim to specific, real examples from DIP (multi-service, multi-language pipeline) or Healthcare Staffing (multi-channel notification services) rather than leaving it abstract.

### 3. Questions Most Likely to Trigger Deep Follow-ups

- **Q7 / Chain 1 (DIP — Led Service Bus migration):** Strongest ownership claim on the resume; expect drill-down into cutover mechanics, specific Service Bus features, and outcome validation.
- **Q8 / Q12 (DIP — Pipeline design and failure handling):** "Designed" and "reliability" claims invite deeper architecture-level follow-ups.
- **Q2 / Q9 / Chain 2 (RAG and LLM integration in GreenShield/DIP):** Technical RAG questions tend to cascade into chunking, retrieval quality, and failure-mode follow-ups.
- **Q5 / Chain 3 (GreenShield business collaboration):** Interviewers often keep probing collaboration claims until they find a concrete example of influence or friction.
- **Q28 (Timeline overlap):** A detail-oriented interviewer noticing the GreenShield/IBM date overlap may keep probing until the explanation is fully clear and consistent.

### 4. Areas Where My Resume Has Limited Detail

(Full detail in Part 6 above — consolidated list.)
- Service Bus cutover mechanics and any real measurable outcome at DIP.
- Pipeline failure/retry handling detail at DIP.
- Specific insurance document types, model/prompt approach, and verification approach at GreenShield.
- A concrete business-requirement example that shaped the GreenShield AI workflow, and its real deployment status.
- Exact real-time notification delivery mechanism at Healthcare Staffing.
- Precise Neudesic → IBM transition timeline (and the GreenShield date overlap).
- Actual hands-on depth for LangChain, LangGraph, AI Agents, Function Calling, Vector Search, Embeddings, Docker, LLM Evaluation.
- A specific feature example and lesson learned from the CDK Global internship.

**I must prepare these from my actual, real experience — none of this should be invented before the interview.**

---

### 5. Final 20-Question Mock Interview

---

**1. Introduction / Career Summary**
**Question:** Can you walk me through your background and how you got to where you are today?
**Model Answer:** I'm a software engineer with about four years of experience, the last two of which have been focused specifically on Generative AI and Microsoft Azure. I started as an intern at CDK Global working on full-stack enterprise features, which turned into a full-time offer. From there I joined Neudesic, working first on a healthcare staffing platform, and then moved into Generative AI work on their Document Intelligence Platform after being promoted to Senior Consultant. Neudesic operates as an IBM company, and I'm currently a Senior Application Consultant at IBM, working on GenAI for insurance claims processing.
*(Full detail: see Q29.)*

**2. Most Recent Role — Current Project**
**Question:** Tell me about what you're working on right now.
**Model Answer:** I'm working on GreenShield, developing and integrating Generative AI capabilities for insurance claims processing and document understanding — using Azure OpenAI, Python, RAG, and Azure AI services to process and extract information from unstructured insurance documents, in collaboration with engineering and business teams.
*(Full detail: see Q1.)*

**3. Technical Deep-Dive on Current Project**
**Question:** How does RAG specifically help with that unstructured document processing on GreenShield?
**Model Answer:** RAG grounds the model's output in the actual document content by retrieving relevant context before generating or extracting information, which makes the output more accurate and traceable back to the source document — important given how much accuracy matters in an insurance claims context.
*(Full detail: see Q2.)*

**4. Challenges / Troubleshooting**
**Question:** What challenges come with extracting information from unstructured insurance documents using GenAI?
**Model Answer:** Insurance documents vary a lot in structure and quality — different formats, terminology, and sometimes scanned or poorly formatted content — so getting consistent, accurate extraction takes real care, since any errors there propagate into downstream claims processing decisions.
*(Full detail: see Q3 — `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` for a specific incident.)*

**5. Model / Prompting Detail**
**Question:** What Azure OpenAI model or prompting approach did you use to extract claim data on GreenShield?
**Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I need to name the actual model and describe the prompting approach (e.g., structured/JSON-output prompting) used to get reliably parseable extraction output.
*(Full detail: see Q4.)*

**6. Cross-Functional Collaboration**
**Question:** How did business teams shape the AI workflow you built on GreenShield?
**Model Answer:** Working with business teams meant understanding which claim data points actually mattered and what counted as acceptable output, which then shaped how I approached extraction and structured the AI workflow, rather than working purely from a technical spec.
*(Full detail: see Q5.)*

**7. Previous, Most Detailed GenAI Project**
**Question:** Before GreenShield, you worked on the Document Intelligence Platform at Neudesic. Tell me about that.
**Model Answer:** DIP was an AI-powered platform used by insurance providers to process claims and extract structured data from large volumes of unstructured documents, using Generative AI and Azure AI services for classification, extraction, and downstream processing. It's the project where I was most involved in actual pipeline design.
*(Full detail: see Q6.)*

**8. Deep Technical Follow-up**
**Question:** You say you led the migration from Azure Storage Queues to Service Bus there — why was that necessary, and what did leading it involve?
**Model Answer:** As the platform needed to support parallel document processing across multiple business units, Storage Queues' more basic messaging model became limiting. I led the change to Service Bus, adapting the processing logic to it and validating that it improved scalability, reliability, and enabled that parallel processing across business units.
*(Full detail: see Q7 and Chain 1.)*

**9. Architecture / Design Question**
**Question:** And you also designed the document processing pipeline itself — what did that design involve?
**Model Answer:** It involved structuring how documents flow through classification and extraction stages using Azure AI services, handling failures at one stage without blocking the whole pipeline, and integrating that with the Service Bus messaging layer so it could handle large volumes reliably.
*(Full detail: see Q8.)*

**10. Service-Level Depth**
**Question:** Azure AI Document Intelligence and Azure Functions are both listed for DIP — how did those fit into the pipeline?
**Model Answer:** Document Intelligence handled the lower-level extraction of structure and fields from raw documents, while Azure Functions orchestrated discrete pipeline steps, event-driven off Service Bus, tying the extraction, LLM understanding, and downstream stages together.
*(Full detail: see Q11.)*

**11. Cross-Stack Integration**
**Question:** DIP used Python, React, Node.js, and .NET — how did those fit together?
**Model Answer:** Python handled the AI/GenAI logic, .NET and Node.js provided backend services and APIs the AI plugged into, and React was the front-end users interacted with — all connected through APIs rather than being one monolithic application.
*(Full detail: see Q10.)*

**12. Earlier Project (Pre-GenAI)**
**Question:** Before moving into GenAI, you worked on a healthcare staffing platform. What did you do there?
**Model Answer:** I was on the Notifications Team, building scalable frontend features in React, TypeScript, and Redux-Saga, and backend services in .NET and C# supporting real-time notification workflows, with a focus on system reliability and user experience.
*(Full detail: see Q13.)*

**13. Technology Rationale**
**Question:** Why Redux-Saga specifically, rather than plain Redux?
**Model Answer:** Redux-Saga gave a more structured way to manage asynchronous side effects — useful in a real-time notification context where you're coordinating multiple async events — compared to handling that logic in a more scattered way with plain Redux.
*(Full detail: see Q14.)*

**14. Skill Evolution**
**Question:** Azure AI Search shows up both in that early project and your recent GenAI work — how has your use of it changed?
**Model Answer:** Early on it was mainly about improving search over communication logs. In my GenAI work, it plays the retrieval role within RAG pipelines — so the same service, but now it's a building block feeding context into a GenAI pipeline rather than a standalone search improvement.
*(Full detail: see Q20.)*

**15. Career Origin**
**Question:** Your career started with an internship at CDK Global — tell me about that.
**Model Answer:** I developed full-stack enterprise features using React, .NET 6, C#, and PostgreSQL, and used tools like SonarQube and Splunk to help maintain quality. That internship led to a full-time offer, which is where my professional career really began.
*(Full detail: see Q17.)*

**16. Skills Probing — Listed but Less-Evidenced**
**Question:** Your skills list LangChain, LangGraph, and AI agent concepts — what's your actual hands-on experience there?
**Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I need to answer honestly here about which of these I've used hands-on in a real project (GreenShield or DIP) versus at a conceptual/learning level.
*(Full detail: see Q23.)*

**17. Claim Validation**
**Question:** Your summary claims you can "design and deliver scalable, production-ready AI applications" — what's your best evidence for that?
**Model Answer:** DIP is the clearest evidence — that's where I designed the document processing pipeline and led the Service Bus migration that made parallel, scalable processing across business units possible.
*(Full detail: see Q27.)*

**18. Production-Readiness Check**
**Question:** Is GreenShield actually live in production, or more of a pilot at this stage?
**Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — I should answer honestly based on the real rollout status of the pieces I built, rather than assuming full production status.
*(Full detail: see Q30.)*

**19. Career Progression / Behavioral**
**Question:** You've moved from intern to Senior Application Consultant fairly quickly, and Neudesic became part of IBM along the way — what drove that progression, and what changed at each step?
**Model Answer:** Each step came with more responsibility — from building features as an intern, to owning specific frontend/backend deliverables as a Consultant, to taking on real design and leadership work, like the Service Bus migration, as a Senior Consultant, and now continuing that GenAI focus at IBM after Neudesic's transition into IBM.
*(Full detail: see Q29.)*

**20. Closing / Forward-Looking**
**Question:** Given everything you've worked on, where do you see your strongest value-add — GenAI, or core software engineering?
**Model Answer:** `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` — this is a genuine reflection question the resume can't answer for me. I should give an honest, considered answer based on where I've felt most effective and where I want to keep growing — likely framing it as GenAI work built on a solid software engineering foundation, since that's what the resume overall supports, but the specific answer should be mine.

---

*End of guide. Review Part 6 and the `[CUSTOMIZE WITH YOUR ACTUAL EXPERIENCE]` markers throughout before the interview — these are the exact gaps between what the resume states and what a real interviewer will expect me to elaborate on from memory.*

## GenAI Quick-Reference Summary Table

| # | Topic | Essential recall |
|---|---|---|
| 1 | Generative AI | Models generate text, structured data, code, images, or other content from instructions and context |
| 2 | LLM | A large language model predicts the next token; it is powerful but can be wrong or outdated |
| 3 | Tokens and context window | Tokens measure input/output; the context window limits how much the model can process in one request |
| 4 | Azure OpenAI | Azure-hosted model endpoints with Azure security, governance, quotas, and monitoring |
| 5 | Prompt engineering | Clear role, task, constraints, examples, context, and expected output format improve consistency |
| 6 | Temperature | Lower values make output more predictable; higher values increase variation and creativity |
| 7 | Structured output | Ask for JSON/schema-shaped output, then validate it before downstream processing |
| 8 | Pydantic/schema validation | Validate LLM output types and required fields; reject or repair invalid responses |
| 9 | Hallucination | A plausible but unsupported model answer; reduce it with grounding, validation, and human review |
| 10 | RAG | Retrieve relevant private/current content and provide it to the model at query time; it does not train the model |
| 11 | Embeddings | Numeric representations of meaning used to compare semantic similarity |
| 12 | Vector search | Finds semantically similar content even when wording differs |
| 13 | Hybrid search | Combines keyword and vector search for stronger recall and precision |
| 14 | Azure AI Search | Common RAG retrieval layer with keyword, vector, hybrid, filters, semantic ranking, and metadata |
| 15 | Chunking | Split documents into useful, coherent pieces; chunk size and overlap affect retrieval quality and cost |
| 16 | Metadata and filters | Store source, document type, tenant, date, and permissions to filter retrieval correctly |
| 17 | Grounding and citations | Base answers on retrieved sources and return citations so output can be checked |
| 18 | Azure AI Document Intelligence | Extract text, tables, fields, and document structure from forms, PDFs, and scanned documents |
| 19 | Document processing pipeline | Ingest document -> extract/classify -> enrich with LLM/RAG -> validate -> send structured result downstream |
| 20 | Azure Functions | Event-driven processing for individual pipeline stages, often triggered by messages or storage events |
| 21 | Service Bus | Durable enterprise queues/topics; supports retries, dead-letter queues, and pub/sub |
| 22 | Storage Queue vs Service Bus | Storage Queue is simple; Service Bus adds topics, subscriptions, sessions, dead-lettering, and richer reliability features |
| 23 | Idempotency | A retried message must not create duplicate business actions or corrupt data |
| 24 | Dead-letter queue | Holds messages that repeatedly fail so they can be investigated or replayed safely |
| 25 | Agents | LLM-based systems that plan and call approved tools to complete a goal |
| 26 | Function/tool calling | The model selects a defined tool and structured arguments; the application validates and executes them |
| 27 | LangChain and LangGraph | Frameworks for composing LLM chains/tools and stateful, multi-step agent workflows |
| 28 | Evaluation | Measure correctness, groundedness, relevance, safety, latency, cost, and task-specific success |
| 29 | Observability | Trace prompts, retrieval, tool calls, model output, latency, failures, and user feedback |
| 30 | Prompt injection | Treat user/retrieved text as untrusted; separate instructions from data and restrict tool permissions |
| 31 | Data security | Enforce tenant/document permissions during retrieval; never put secrets into prompts or logs |
| 32 | Responsible AI | Consider safety, privacy, fairness, transparency, human review, and escalation paths |
| 33 | Fine-tuning vs RAG | Fine-tuning changes model behavior; RAG supplies current/private knowledge at runtime |
| 34 | Cost and performance | Track tokens, embeddings, search calls, and latency; cache safe results and choose fit-for-purpose models |
| 35 | GreenShield pattern | Use GenAI/RAG to ground insurance document understanding and feed validated data into claims workflows |
| 36 | DIP pattern | Use Document Intelligence, Azure OpenAI, AI Search, Functions, and Service Bus for scalable document processing |
| 37 | Full-stack role split | React = user experience; .NET/Node = APIs/workflows; Python = AI/LLM processing; APIs connect the pieces |

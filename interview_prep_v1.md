# Interview Revision Cheat Sheet — Senior SWE / GenAI Engineer / Full Stack

Profile basis: 4+ yrs SWE, 2+ yrs GenAI & Azure. Projects: AWS→Azure Modernization (CaseWorthy), Document Intelligence Platform (GreenShield/DIP), Healthcare Staffing Platform, CDK Full Stack App.

Format note: each topic has a plain-English explanation, then light tags — *proj* (where it connects to my work), *Q* (likely follow-up), *A* (short answer).

---

## SECTION 1: SOFTWARE ARCHITECTURE

### Monolithic Architecture
Everything — UI, logic, data access — lives in one deployable app. Easy to build and deploy at first, but gets harder to manage as the app and team grow.
*proj: (would-apply) the CDK Global app (React + .NET 6 + PostgreSQL) was closer to this style.*
*Q: When would you avoid a monolith?*
*A: When parts of the system need to scale or deploy independently, or use different tech — then microservices fit better.*

### Modular Monolith
One deployable app, but split internally into clean, separate modules. You get organized code without the extra work of running many services.
*proj: (would-apply) good stepping stone before breaking a monolith into microservices.*
*Q: Why not just start with microservices?*
*A: Early on, microservices add network calls, deployment overhead, and data-consistency headaches you don't need yet.*

### Microservices Architecture
The app is split into small, independently deployable services, each owning its own data and job. You can scale and deploy each piece on its own, but you take on more moving parts.
*proj: (direct) DIP had separate services/functions for ingestion, classification, extraction, and search indexing, talking over Azure Service Bus.*
*Q: What are the downsides?*
*A: More operational work — tracing requests across services, handling network failures, keeping data consistent, and versioning deployments.*

### Layered Architecture
Code split into horizontal layers — UI, business logic, data access — each only talking to the layer below it.
*proj: (direct) .NET services in the Healthcare Staffing Platform followed Controller → Service → Repository.*
*Q: What's the risk?*
*A: If business logic directly depends on concrete data access code, it's hard to test or swap the data source — Dependency Inversion fixes that.*

### Clean Architecture
Business rules sit at the center; frameworks, DBs, and UI sit on the outside, and everything depends inward toward the core. Keeps core logic independent of any specific tech.
*proj: (would-apply) fits how DIP's extraction logic could stay decoupled from Azure SDKs directly.*
*Q: How does it help testing?*
*A: Core logic has no framework dependency, so you can unit test it without a real DB or web server.*

### Onion Architecture
Basically the same idea as Clean Architecture — domain at the center, infrastructure on the outer rings, dependencies point inward.
*proj: (would-apply) pattern for keeping domain entities free of EF/Dapper types.*
*Q: Onion vs Clean — what's the difference?*
*A: Almost none conceptually — naming and ring layout differ slightly, the goal is the same.*

### Hexagonal Architecture (Ports & Adapters)
The core exposes "ports" (interfaces); outside systems plug in through "adapters." This keeps the core isolated from any specific tech.
*proj: (direct-ish) DIP's queue-consumer abstraction let us swap Storage Queues for Service Bus with barely any change to core logic.*
*Q: How's this different from Clean/Onion?*
*A: Same goal of isolating business logic — Hexagonal just frames it as ports/adapters instead of rings.*

### Domain-Driven Design (DDD)
Design the software around the real business domain, using the same language the business uses. Helps split big systems into clear "bounded contexts."
*proj: (would-apply) DIP's split between document ingestion, classification, and claims extraction maps well to this idea.*
*Q: What's a "bounded context"?*
*A: A boundary where a term means one specific thing — e.g., "Document" means something different in ingestion vs. billing.*

### Separation of Concerns
Each part of the system handles one job, without overlapping responsibilities.
*proj: (direct) kept React display components separate from Redux-Saga side-effect logic in the Healthcare Staffing Platform.*
*Q: How does this relate to SRP?*
*A: SRP is this same idea, just applied at the class/function level.*

### Loose Coupling
Components talk through interfaces/contracts, not by depending on each other's internals.
*proj: (direct) used interfaces for notification channels in the Healthcare Staffing Platform's multi-channel workflows.*
*Q: How do you get loose coupling in .NET?*
*A: Code against interfaces, inject dependencies via DI, and use events/messages instead of direct calls where you can.*

### High Cohesion
Everything inside a module is closely related and focused on one clear purpose.
*proj: (direct) each Azure Function in DIP did one job — classify, extract, or index.*
*Q: Example of low cohesion?*
*A: A "Utils" class doing logging, email, and DB access all at once — unrelated things crammed together.*

### Dependency Injection
Instead of a class creating its own dependencies, they're handed to it from outside (usually by a container).
*proj: (direct) used ASP.NET Core's built-in DI container across DIP and the Healthcare Staffing Platform for repositories, HTTP clients, Azure SDK clients.*
*Q: DI vs IoC — what's the difference?*
*A: IoC is the broader idea of inverting control flow; DI is one way to do IoC, by injecting dependencies.*

### Dependency Inversion
High-level code should depend on abstractions (interfaces), not on concrete low-level code.
*proj: (direct) repository interfaces abstracted SQL Server/Dapper and Cosmos DB access in the Healthcare Staffing Platform.*
*Q: Isn't this the same as DI?*
*A: DI is the mechanism; Dependency Inversion is the design goal DI helps you reach.*

### Repository Pattern
Hides data access behind an interface so business logic never talks to the DB directly.
*proj: (direct) repository abstractions over Dapper/SQL Server and Cosmos DB in the Healthcare Staffing Platform.*
*Q: Isn't this redundant with EF Core's DbContext?*
*A: Fair point for simple cases — I'd only add a repository layer to abstract across multiple data sources or make testing easier.*

### Unit of Work
Tracks changes across several operations and commits them together as one transaction.
*proj: (would-apply) EF Core's `DbContext` already works this way — `SaveChanges()` commits everything tracked, atomically.*
*Q: What about a unit of work spanning two databases, like SQL Server + Cosmos DB?*
*A: You can't get one true ACID transaction across both — use the Saga or outbox pattern for eventual consistency instead.*

### CQRS (Command Query Responsibility Segregation)
Splits read operations from write operations, sometimes with different models for each.
*proj: (would-apply) fits DIP, since writes (ingestion) and reads (Azure AI Search queries) scale very differently.*
*Q: When would you skip CQRS?*
*A: For simple CRUD apps — it adds complexity that's not worth it unless reads and writes genuinely diverge.*

### Event-Driven Architecture
Services publish and react to events instead of calling each other directly.
*proj: (direct) DIP's pipeline used Azure Service Bus events to kick off classification, extraction, and indexing.*
*Q: What's a challenge here?*
*A: Tracing one request across many event handlers is harder — you need distributed tracing and correlation IDs.*

### Event Sourcing
Instead of storing the current state, you store every event that happened, and rebuild state by replaying them.
*proj: (would-apply) not something I've fully built, but similar in spirit to how audit logs worked in notification workflows.*
*Q: What's the downside?*
*A: More complexity — you need event versioning, and reading "current state" needs replay or maintained projections.*

### API Gateway
One entry point that routes client requests to the right backend service, and often handles auth/rate limiting/logging centrally.
*proj: (would-apply) natural fit for putting DIP's multiple backend services behind one Azure API Management/App Service front door.*
*Q: What if the gateway goes down?*
*A: It's a single point of failure — mitigate with high-availability deployment, health checks, and autoscaling.*

### Service Discovery
How services find each other's network location automatically instead of using hardcoded addresses.
*proj: (would-apply) Azure Functions/App Service handle this via built-in DNS routing, so I haven't had to build it manually.*
*Q: How does Kubernetes do this?*
*A: Internal DNS — each Service gets a stable name that load-balances across healthy pods.*

### "Explain the architecture of your application" — DIP + AWS→Azure
*A:* "On the Document Intelligence Platform, we built an event-driven pipeline for processing large volumes of unstructured insurance documents. Documents land in Azure Blob Storage, which triggers a message onto Azure Service Bus. Several Azure Functions consume these messages independently — one classifies the document type, another calls Azure AI Document Intelligence for structured extraction, and another uses Azure OpenAI with RAG (backed by Azure AI Search) for unstructured, free-text sections. Results go into a database and get indexed into Azure AI Search for retrieval. We moved this pipeline from Storage Queues to Service Bus specifically for topics/subscriptions (parallel processing across business units), dead-letter queues, and better throughput. On the AWS→Azure side, I worked on remediating Mirth Connect integrations and re-pointing AWS-dependent connectors to Azure Blob Storage, Key Vault, and Service Bus, using Managed Identity so services authenticate without embedded secrets."

---

## SECTION 2: SOLID PRINCIPLES

### Single Responsibility Principle (SRP)
A class should have only one reason to change — one job.
```csharp
// Bad: handles both business logic and persistence
class OrderService {
    void PlaceOrder(Order o) { /* validate */ SaveToDb(o); }
}
// Good: split responsibilities
class OrderValidator { void Validate(Order o) { } }
class OrderRepository { void Save(Order o) { } }
```
*ex: a `ClaimProcessor` shouldn't also parse Service Bus messages AND write to the DB AND send emails — split those out.*
*why it helps: smaller, focused classes are easier to test alone and less likely to break other things when changed.*
*Q: How do you spot SRP violations in existing code?*
*A: Look for classes with lots of unrelated methods, or ones that change for many unrelated business reasons — that's the signal to split.*

### Open/Closed Principle (OCP)
You should be able to add new behavior without editing existing, working code.
```csharp
interface IDiscountStrategy { decimal Apply(decimal price); }
class SeasonalDiscount : IDiscountStrategy { public decimal Apply(decimal p) => p * 0.9m; }
// Add new discount types without modifying existing code
class LoyaltyDiscount : IDiscountStrategy { public decimal Apply(decimal p) => p * 0.85m; }
```
*ex: adding a new document classifier type in DIP without touching the existing classification pipeline — just register a new handler.*
*why it helps: less regression risk, since tested code paths stay untouched when you add features.*
*Q: How do you apply this in .NET?*
*A: Interfaces/strategy pattern plus DI, so new implementations plug in via registration instead of editing existing classes.*

### Liskov Substitution Principle (LSP)
A subclass should be usable anywhere its base class is used, without breaking things.
```csharp
class Rectangle { public virtual int Width { get; set; } public virtual int Height { get; set; } }
class Square : Rectangle { } // violates LSP if it forces Width==Height unexpectedly
```
*ex: if `INotificationSender` implementations (Email, SMS, Push) all promise "send and return success/failure," none should throw surprise exceptions callers don't expect.*
*why it helps: makes polymorphism safe — code using the interface works no matter which implementation is plugged in.*
*Q: Give an example of an LSP violation you avoided.*
*A: Avoided a subclass overriding a method to throw `NotImplementedException` for a case the base class supported — that breaks callers relying on the base contract.*

### Interface Segregation Principle (ISP)
Don't force a class to depend on methods it doesn't need — keep interfaces small and specific.
```csharp
// Bad
interface IWorker { void Work(); void Eat(); }
// Good
interface IWorkable { void Work(); }
interface IFeedable { void Eat(); }
```
*ex: splitting a bloated `IDocumentService` into `IDocumentClassifier`, `IDocumentExtractor`, `IDocumentIndexer` so each Azure Function only depends on what it uses.*
*why it helps: less mocking work in tests, less coupling to code you don't use.*
*Q: How do you refactor a fat interface?*
*A: Group methods by which client uses them, then split into separate interfaces along those lines.*

### Dependency Inversion Principle (DIP)
Depend on interfaces, not concrete classes — both high-level and low-level code should rely on abstractions.
```csharp
interface IEmailSender { void Send(string to, string body); }
class NotificationService {
    private readonly IEmailSender _sender;
    public NotificationService(IEmailSender sender) { _sender = sender; } // injected abstraction
}
```
*ex: business logic depends on an `IDocumentStorage` interface, not directly on `BlobServiceClient`, so storage can be swapped or mocked.*
*why it helps: easy to mock in unit tests, and easy to swap infrastructure with minimal code change.*
*Q: How is this enforced in ASP.NET Core?*
*A: Through the built-in DI container — register interface-to-implementation in `Program.cs`, then inject interfaces into constructors.*

### "Where have you used SOLID principles in your application?"
*A:* "In the Healthcare Staffing Platform's .NET backend, we used interfaces for notification channels (email, SMS, push) — each implementing a common `INotificationSender` interface (ISP + DIP), injected via constructor DI so we could add new channels without touching existing dispatch logic (OCP). Repository classes only handled data access, kept separate from business/validation logic (SRP). That made the notification pipeline testable — we could mock `INotificationSender` in unit tests without hitting real email/SMS providers."

---

## SECTION 3: SYSTEM DESIGN

### Scalability
Handling more load (users, data, requests) without slowing down.
*use it when: any system expected to grow — design for it, but don't over-build for scale you don't need yet.*
*proj: (direct) DIP had to handle large volumes of insurance documents across multiple business units at once.*

### Horizontal Scaling
Adding more instances/machines instead of making one machine bigger.
*use it when: you can run several stateless instances behind a load balancer — the cloud-native default (Azure Functions, App Service scale-out).*
*proj: (direct) Azure Functions in DIP scaled out to multiple instances processing Service Bus messages in parallel.*

### Vertical Scaling
Making a single machine bigger (more CPU/RAM) instead of adding more machines.
*use it when: quick fix for one bottleneck, but it has a hard ceiling and usually needs downtime to resize.*
*proj: (would-apply) bumping a SQL Server tier before considering read replicas/sharding.*

### Load Balancing
Spreads incoming traffic across instances so no single one gets overloaded.
*use it when: any horizontally scaled service — Azure App Service/Front Door does this for you automatically.*
*proj: (would-apply) Azure Functions/App Service instances behind Azure's built-in load balancing.*

### Stateless Services
Any instance can handle any request because nothing important is kept in memory — easy to scale and recover.
*use it when: default for APIs/microservices — keep session/state in a DB, cache, or token instead.*
*proj: (direct) Azure Functions in DIP were stateless; state lived in Blob Storage, the DB, or Service Bus messages.*

### Stateful Services
An instance holds onto in-memory context (like a WebSocket connection) that has to be preserved.
*use it when: real-time collaboration, gaming, or SignalR-based notifications — needs sticky sessions or shared state.*
*proj: (would-apply) real-time notification workflows in the Healthcare Staffing Platform may have needed sticky connections for delivery.*

### High Availability
Minimizing downtime by removing single points of failure.
*use it when: production systems with uptime targets — multi-instance deployments, health checks, geo-redundancy.*
*proj: (would-apply) relying on Azure App Service/Functions' built-in multi-instance redundancy.*

### Fault Tolerance
The system keeps working (maybe in a reduced way) even when a piece fails.
*use it when: any distributed system — combine retries, circuit breakers, redundancy.*
*proj: (direct) Service Bus retry + dead-letter handling in DIP meant one bad document didn't stop the whole pipeline.*

### Reliability
The system keeps giving correct results over time.
*use it when: always the goal; measured with error rates/SLAs.*
*proj: (direct) an explicit focus for the Healthcare Staffing Platform's notification workflows.*

### Resilience
How well a system bounces back from failure.
*use it when: combine retry, circuit breaker, bulkhead, and DLQ patterns together.*
*proj: (direct) DIP's Service Bus pipeline used retries and a DLQ for bad messages.*

### Caching
Store data you'll need again so you don't re-fetch/re-compute it every time — cuts latency and load.
*use it when: read-heavy work where data doesn't change every request.*
*proj: (would-apply) caching embeddings or common Azure AI Search results to cut OpenAI/Search calls and cost.*

### Distributed Cache
A shared cache (like Redis) usable by all your service instances, instead of each instance caching separately.
*use it when: multi-instance deployments that need the same cached data everywhere.*
*proj: (would-apply) Redis to cache Azure AI Search results or session tokens across scaled-out instances.*

### CDN
Serves static files from servers near the user, so pages load faster.
*use it when: static content (JS/CSS/images) for a React app.*
*proj: (would-apply) Azure CDN/Front Door in front of a React app's static assets.*

### Rate Limiting
Caps how many requests a client can make in a time window, to stop abuse/overload.
*use it when: public APIs, or protecting costly calls like Azure OpenAI.*
*proj: (would-apply) throttling Azure OpenAI calls to control cost and stay under quota.*

### Database Scaling
Handling more data/queries than one DB instance can manage.
*use it when: read/write load or storage outgrows a single instance — via replication, partitioning, or sharding.*
*proj: (would-apply) considered for SQL Server under heavy claims/document volume.*

### Database Replication
Copies data to secondary nodes to improve read speed and availability.
*use it when: read-heavy workloads or disaster-recovery needs — replicas serve reads while the primary handles writes.*
*proj: (would-apply) SQL Server read replicas for reporting queries, so they don't slow down transactional writes.*

### Database Partitioning
Splits a big table into smaller pieces (like by date) to keep queries fast.
*use it when: very large tables where queries can target one partition, like monthly records.*
*proj: (would-apply) partitioning claims/documents by date or business unit.*

### Sharding
Splits data across multiple database instances based on a key, so no single DB holds everything.
*use it when: one DB instance can't handle the total data or write load — common with Cosmos DB partition keys.*
*proj: (direct) designed Cosmos DB partition keys in the Healthcare Staffing Platform to spread data across partitions.*

### CAP Theorem
In a distributed system you can only really guarantee two of: Consistency, Availability, Partition tolerance — and since networks do fail, it comes down to choosing Consistency vs. Availability.
*use it when: picking a Cosmos DB consistency level (strong vs. eventual) based on whether you need strict correctness or speed/availability.*

### Strong Consistency
Every read always sees the latest write — important when stale data would cause real problems.
*use it when: things like claim status updates, where correctness matters more than speed.*
*proj: (would-apply) SQL Server transactions default to this for claims records.*

### Eventual Consistency
Trades instant consistency for better availability/speed — everything catches up eventually.
*use it when: search indexes, caches, or cross-service data where a small lag is fine.*
*proj: (direct) Azure AI Search index in DIP/Healthcare Staffing lagged slightly behind the source DB until indexing jobs ran.*

### Idempotency
Running the same operation once or many times gives the same result.
This matters a lot for message consumers and retries — reprocessing the same Service Bus message shouldn't create a duplicate claim record.
*proj: (direct) message handlers in DIP checked the document ID before processing, so retries didn't duplicate extraction.*
*Q: How do you make a consumer idempotent?*
*A: Use a unique ID, check if it's already been processed (dedup table or upsert), and write with upserts instead of blind inserts.*

### Message Queues
Decouples producers and consumers by buffering work, so producers don't wait on consumers.
*use it when: any long or bursty job that shouldn't block the caller, like document processing.*
*proj: (direct) Azure Service Bus (and before that, Storage Queues) in DIP.*

### Pub/Sub
Multiple independent subscribers can react to the same event without the publisher knowing who's listening.
*use it when: several business units/consumers need the same event — Service Bus Topics/Subscriptions.*
*proj: (direct) Service Bus topics let multiple business units process DIP events in parallel.*

### Event-Driven Architecture (system design lens)
Async, decoupled processing instead of one long synchronous chain of calls.
*use it when: long workflows, multi-step pipelines, or consumers that need to scale independently.*
*proj: (direct) the whole DIP ingestion-to-indexing pipeline.*

### Synchronous vs Asynchronous Communication
Sync (REST) gives an instant answer but blocks/couples caller and callee; async (queues/events) decouples them but adds eventual-consistency complexity.
*use it when: sync for things needing an immediate answer (login); async for long or fan-out work (document processing).*
*proj: (direct) React calls REST APIs synchronously, but document processing runs asynchronously via Service Bus.*

### Retry Pattern
Automatically retries a failed call (with backoff) instead of giving up right away — good for short-lived, transient failures.
*use it when: calling external services (Azure OpenAI, Document Intelligence, DB) that occasionally hiccup.*
*proj: (direct) retry policies around Azure AI calls and Service Bus processing in DIP.*

### Circuit Breaker
Stops hammering a failing service so it gets time to recover, instead of piling up retries.
*use it when: a downstream dependency (like Azure OpenAI) is degraded — fail fast instead of retrying forever.*
*proj: (would-apply) would wrap Azure OpenAI/Document Intelligence calls with Polly's circuit breaker in .NET.*

### Bulkhead Pattern
Isolates resources (thread pools, connections) per component, so one failing dependency can't starve the others.
*use it when: separating connection pools for AI calls vs. DB calls, so a slow AI call doesn't block DB access.*
*proj: (would-apply) isolating extraction workers from indexing workers in DIP.*

### Dead Letter Queue (DLQ)
A side queue where messages that keep failing get moved, so they don't block the main queue.
*use it when: you need to inspect/reprocess "poison messages" (a malformed document) without stalling the pipeline.*
*proj: (direct) Azure Service Bus DLQ in DIP for documents that failed classification/extraction after retries.*
*Q: What do you do with messages in the DLQ?*
*A: Monitor/alert on DLQ depth, then inspect and reprocess or fix the root cause before replaying.*

### Observability
Being able to tell what's happening inside a system just from its logs, metrics, and traces.
*proj: (direct) Application Insights/Azure Monitor across DIP services.*

### Logging
Records individual events for debugging and audit.
*proj: (direct) structured logging via Application Insights in .NET/Python services.*

### Monitoring
Tracks system health/metrics over time and alerts when something looks off.
*proj: (direct) Azure Monitor dashboards/alerts, and Splunk at CDK Global.*

### Distributed Tracing
Follows one request/message across multiple services to see where time was spent or where it failed.
*use it when: any multi-service async pipeline — key for debugging DIP's Service Bus flow.*
*proj: (would-apply) correlation IDs through Service Bus messages plus Application Insights for end-to-end tracing.*

### "How would you design a scalable document processing system?"
*A:* "I'd design it as an event-driven pipeline. Clients upload documents via an API that writes them straight to Azure Blob Storage and publishes a message to Azure Service Bus with a document reference and correlation ID — keeps the API fast and stateless. Several consumer Azure Functions subscribe to Service Bus topics: one calls Azure AI Document Intelligence for structured extraction, another uses Azure OpenAI with RAG over Azure AI Search for unstructured content. Each consumer is idempotent — it checks a processing-status table by document ID before doing work, so retries don't duplicate anything. Azure Functions auto-scale based on queue length, giving horizontal scalability. For failures, Service Bus retries with backoff, and after max retries messages land in a dead-letter queue that we monitor and alert on. Results go to the database and get indexed into Azure AI Search. Throughout, we use Application Insights with correlation IDs for tracing a document's journey end-to-end, plus dashboards on queue depth, latency, and DLQ counts."

---

## SECTION 4: MICROSERVICES AND DISTRIBUTED SYSTEMS

### Microservices
See Section 1. Key trade-off: independent scaling/deployment vs. more operational complexity.

### Inter-Service Communication
How independently deployed services exchange data.
*use it when: REST for direct request/response, messaging for async/decoupled work.*
*proj: (direct) REST between React and .NET; Service Bus messaging between DIP's processing stages.*

### REST Communication
Simple, synchronous, HTTP-based calls between services or client-to-service.
*use it when: the caller needs an answer right away, like fetching search results.*
*proj: (direct) all .NET Web APIs across projects.*

### Message-Based Communication
Decouples services in time — the producer doesn't need the consumer online right now.
*use it when: long, async, or fan-out work like document processing.*
*proj: (direct) Azure Service Bus in DIP.*

### Synchronous vs Asynchronous Communication
Same idea as Section 3: sync = simpler but blocking; async = resilient/scalable but eventually consistent.

### Database per Service
Each service owns its own data model instead of sharing one schema, avoiding tight coupling.
*use it when: true microservices needing independent deployability/scaling.*
*proj: (would-apply) DIP used different stores for different needs (relational for transactions, Azure AI Search for retrieval) instead of one shared schema.*

### Distributed Transactions
Coordinating one transaction across multiple services/databases.
*use it when: rarely — avoid true 2-phase-commit transactions and use Saga instead for cross-service consistency.*

### Saga Pattern
A multi-step distributed transaction handled as a chain of local transactions, each with a "undo" step if something later fails.
Since you can't do one ACID transaction across services, Saga uses events (or a coordinator) to keep data eventually consistent, rolling back via compensating actions on failure.
*proj: (would-apply) fits a claims workflow spanning classification → extraction → indexing, where a downstream failure needs upstream cleanup.*
*Q: Choreography vs Orchestration Saga — which would you pick?*
*A: Choreography (event-driven, no central controller) for simple flows like DIP's pipeline; orchestration (central coordinator) when the workflow has complex branching that's hard to follow via events alone.*

### Eventual Consistency
Covered in Section 3 — key point here: since each service has its own DB, the cross-service view of data is eventually, not immediately, consistent.

### Service Discovery / API Gateway / Circuit Breaker / Retry / Dead Letter Queue
Covered above — same ideas, applied directly at the microservices communication layer.

### Idempotent Consumers
Consumers built so processing the same message twice has no bad side effects.
Needed in at-least-once systems like Service Bus, where redelivery can happen after a crash or timeout.
*proj: (direct) DIP's Service Bus consumers checked document processing status before re-running extraction.*

### Distributed Tracing
Covered in Section 3.

### "Why would you choose microservices over a monolith?"
*A: When different parts of the system have very different scaling needs, release schedules, or team boundaries — e.g., DIP's classification, extraction, and search indexing scale independently and can deploy separately without redeploying everything.*

### "When would you prefer a monolith?"
*A: Smaller teams, simpler domains, or early-stage products where distributed-system overhead isn't worth it yet — start monolith, split out services as boundaries get clearer.*

### "How do microservices communicate?"
*A: Synchronously via REST/gRPC for immediate answers, or asynchronously via a broker like Azure Service Bus for decoupled, resilient work — I've used REST for client-facing APIs and Service Bus for backend pipelines.*

### "How do you maintain data consistency in distributed systems?"
*A: Favor eventual consistency with idempotent operations and the Saga pattern for multi-step workflows, use correlation IDs to trace a transaction across services, and save strong consistency/ACID for single-service, single-database operations.*

---

## SECTION 5: .NET AND C#

### CLR (Common Language Runtime)
The runtime that executes .NET code — handles JIT compilation, memory management, and type safety.
All .NET languages compile to IL, which the CLR JIT-compiles to native code at runtime and manages via garbage collection.

### CTS (Common Type System)
Defines how types are declared and used across all .NET languages, so C# and F# can share types.

### CLS (Common Language Specification)
A subset of CTS rules that guarantees cross-language compatibility — a CLS-compliant public API can be used from any .NET language.

### Managed vs Unmanaged Code
Managed code runs under the CLR (GC, type safety); unmanaged code (e.g., native C++) manages its own memory.
`IDisposable`/`using` matters specifically for unmanaged resources (file handles, DB connections, sockets) that the GC won't clean up on its own.

### Garbage Collection
Automatic memory management that frees memory for objects the app can no longer reach.
The GC compacts the heap and moves surviving objects between generations — less manual memory work, but with pause-time trade-offs.

### GC Generations
Heap is split into Gen 0 (short-lived), Gen 1 (medium), Gen 2 (long-lived); Gen 0 gets collected most often since most objects die young.
This makes GC cheap on average — scanning just Gen 0 frequently is far cheaper than scanning the whole heap every time.

### IDisposable
An interface (`Dispose()`) for classes holding unmanaged resources, so they can be released on a predictable schedule.
Used for DB connections, file streams, HTTP clients — anything the GC can't reliably clean up on its own timeline.

### using
Syntax that guarantees `Dispose()` runs even if an exception happens, via a compiler-generated `try/finally`.
I use `using` for `SqlConnection`, `HttpClient` (careful with reuse), streams, etc., to avoid leaks.

### async and await
Lets you write non-blocking async code that reads like normal sync code, freeing the thread while waiting on I/O.
Used a lot in ASP.NET Core APIs and Azure Functions to avoid blocking threads during DB calls, Azure OpenAI calls, or Service Bus operations — important for scaling under load.

### Task
Represents an async operation that may finish later, optionally with a result (`Task<T>`).
The building block behind async/await — I return `Task<IActionResult>` from controller actions and `Task` from async service methods.

### Thread
An OS-level unit of execution; `Task` is a higher-level wrapper that may or may not use a dedicated thread.
I rarely manage raw `Thread` objects — the Task Parallel Library and async/await abstract this away for most app code.

### Thread Pool
A managed pool of reusable worker threads .NET uses to run queued work (including async continuations), so it doesn't create a new thread per task.

### Parallel Programming
Running independent operations at the same time using things like `Parallel.ForEach` or `Task.WhenAll`, to get more throughput.
I'd use `Task.WhenAll` to process multiple documents/chunks at once (like calling the embeddings API for several chunks in parallel) instead of awaiting each one in sequence.

### CancellationToken
A way to cooperatively signal that an async operation should be canceled.
Passed through async method chains (from an ASP.NET Core request or Azure Function) so long calls like OpenAI requests can be canceled if the client disconnects or a timeout hits.

### IEnumerable
A sequence you can iterate lazily, one item at a time, usually in-memory (LINQ-to-Objects).
Deferred execution — the query doesn't run until you actually enumerate it (like in a `foreach`).

### IQueryable
Like `IEnumerable`, but the query gets translated (e.g., to SQL) and run at the data source, not in memory.
With EF Core, filtering/sorting on `IQueryable` becomes SQL run in the database; `IEnumerable` would pull everything into memory first — a classic performance trap interviewers ask about.

### IAsyncEnumerable
An async version of `IEnumerable` — you `await foreach` over items produced asynchronously (streamed from a DB or API).
Useful for streaming large result sets (like paged documents) without loading everything into memory at once.

### Value Type vs Reference Type
Value types (`struct`, `int`, `bool`) live inline (usually on the stack) and copy by value; reference types (`class`) live on the heap and are accessed by reference.
Passing a large struct around by value can be costly (copying); passing a class just copies a reference — affects both performance and mutation behavior.

### Stack vs Heap
The stack holds value types and method call frames, fast LIFO allocation; the heap holds reference type instances and is managed by the GC.
Stack memory is freed automatically when a method returns; heap memory needs garbage collection, so lots of heap allocations can cause GC pressure.

### String vs StringBuilder
`string` is immutable — every "change" makes a new string; `StringBuilder` is mutable and efficient for repeated concatenation.
I use `StringBuilder` for building large strings in a loop (like assembling a prompt from many chunks) to avoid the overhead of repeated string concatenation.

### var vs dynamic
`var` is statically typed at compile time (type inferred); `dynamic` is resolved at runtime, skipping compile-time checks.
I default to `var` for readability with compile-time safety; `dynamic` is for edge cases like COM interop or truly dynamic JSON shapes, and I avoid it otherwise since it loses IntelliSense/compile-time errors.

### ref, out, and in
`ref` passes a variable by reference (must already be initialized); `out` also passes by reference but doesn't need prior initialization (must be assigned inside the method); `in` passes by reference as read-only (perf boost for large structs).
`out` shows up a lot in `TryParse`-style methods (`int.TryParse(s, out var result)`); I rarely need `ref`/`in` in typical business/web code.

### Exception Handling
Using `try/catch/finally` to handle errors without crashing the app.
I catch specific exception types I can meaningfully handle (like `SqlException` for transient DB errors to retry), and let unexpected exceptions bubble up to global handling/logging.

### Global Exception Handling
Centralized middleware that catches unhandled exceptions app-wide and returns a consistent error response.
In ASP.NET Core, I do this via exception-handling middleware (or `UseExceptionHandler`) that logs (Application Insights) and returns a standard error payload instead of leaking stack traces.

### Middleware
Components in the ASP.NET Core request pipeline that process requests/responses in order (auth, logging, exception handling, etc.).
Each one can short-circuit or pass control along — I've set up pipelines with authentication, CORS, exception handling, and custom logging middleware in that order.

### Dependency Injection / Service Lifetimes
ASP.NET Core's built-in DI container supports three lifetimes: Singleton (one instance for the app's life), Scoped (one per request), Transient (new instance every time).
I use Scoped for `DbContext`/repositories (per-request consistency), Singleton for stateless, thread-safe services like config, and Transient for lightweight, stateless services.
*Q: What happens if you inject a Scoped service into a Singleton?*
*A: It becomes a captive dependency — the scoped service gets "stuck" at singleton lifetime and never refreshes per-request; ASP.NET Core throws an error in development to catch this.*

### Configuration / Options Pattern
Options Pattern binds strongly-typed config classes (like `AzureOpenAIOptions`) from `appsettings.json`/env vars via `IOptions<T>`.
I use this to avoid scattering `IConfiguration["Key"]` string lookups everywhere, and to validate required settings at startup.

### Logging
Structured logging via `ILogger<T>`, usually routed to Application Insights/Azure Monitor.
I use structured/semantic logging (named parameters, not string concatenation) so logs are queryable by things like document ID or correlation ID.

### LINQ
Lets you write SQL-like queries directly in C# over collections, EF Core entities, or XML.
Used a lot across projects to filter/project data both in-memory and translated to SQL via EF Core.

### Deferred Execution
LINQ queries don't run when you write them — only when you enumerate them (`foreach`, `.ToList()`, etc.).
A common gotcha — changing the underlying collection or a captured variable before enumeration changes the query's results; I materialize (`.ToList()`) when I need to "freeze" results at a point in time.

---

## SECTION 6: ASP.NET CORE AND WEB APIs

### ASP.NET Core Request Pipeline
An ordered chain of middleware that each request passes through before reaching the endpoint (and back through on the way out).
Order matters — exception handling should wrap everything, authentication before authorization, CORS before routing.
*proj: (direct) configured pipelines in .NET Web APIs across DIP and the Healthcare Staffing Platform.*

### Middleware
Covered above.

### REST APIs
An architectural style using HTTP verbs and resource-based URLs for stateless client-server communication.
*proj: (direct) REST APIs across all .NET backend projects.*

### HTTP Methods
GET (read), POST (create), PUT (replace), PATCH (partial update), DELETE (remove).
*Q: Which HTTP methods are idempotent?*
*A: GET, PUT, DELETE, and HEAD — repeating them has the same effect as doing it once; POST and PATCH usually aren't.*

### HTTP Status Codes
Quick list: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 409 Conflict, 429 Too Many Requests, 500 Internal Server Error, 503 Service Unavailable.

### Authentication vs Authorization
Authentication checks who you are; Authorization checks what you're allowed to do.
*proj: (direct) JWT-based authentication with role/claims-based authorization in .NET APIs.*

### JWT
A signed, self-contained token carrying claims (identity, roles) that the server can check without a DB lookup.
Used for stateless authentication — the token's signature and expiry get validated on every request.

### OAuth 2.0
An authorization framework letting a third party get limited access to a resource on a user's behalf, without sharing credentials.

### OpenID Connect
An identity layer on top of OAuth 2.0 that adds authentication (an ID token proving who the user is), not just authorization.

### Role-Based Authorization
Access control based on a user's assigned role (Admin, Nurse, Reviewer).

### Policy-Based Authorization
Access control based on custom logic that can combine several claims/conditions — more flexible than plain roles.

### Claims-Based Authorization
Access decisions based on claims in the identity token (department, permission level), not just one role string.

### CORS
A browser rule that restricts which origins can call your API from client-side JS, controlled by server headers.
Configured explicitly in ASP.NET Core to allow the React frontend's origin to call the API.

### API Versioning
A strategy (URL path, query string, or header) for evolving APIs without breaking existing clients.

### Model Validation
Validating incoming request payloads (data annotations or FluentValidation) before processing, returning 400 for bad input.

### Global Exception Handling
Covered in Section 5.

### Rate Limiting
Caps how many requests a client can make in a time window, to protect the API and downstream services.
Especially important for AI-backed endpoints to control Azure OpenAI cost/quota use.

### Swagger / OpenAPI
Auto-generated, interactive API docs/spec built from your controller/endpoint definitions.

### Health Checks
Endpoints (`/health`) reporting whether the app and its dependencies (DB, Service Bus) are healthy, used by load balancers/orchestrators.

### Logging / API Performance
Covered above — key levers: async I/O, caching, pagination, smaller payloads, connection pooling.

### "How are you handling authentication and authorization in your application?"
*A:* "We use JWT-based authentication — the identity provider issues a token with claims like user ID, role, and department after login. ASP.NET Core validates the token's signature and expiry via the authentication middleware, then we use policy-based/claims-based authorization to control access — for example, only users with a 'ClaimsReviewer' claim can access certain document review endpoints. For Azure-to-Azure communication, like Functions calling Key Vault or Blob Storage, we use Managed Identity instead of connection strings or secrets, so there's no credential to leak, and access is governed by RBAC role assignments scoped to exactly what each service needs."

---

## SECTION 7: AZURE CLOUD ARCHITECTURE

### Azure App Service
A fully managed PaaS for hosting web apps/APIs without managing servers, with built-in scaling, deployment slots, and easy CI/CD.

### Azure Functions
Serverless compute that runs event-triggered code (HTTP, Service Bus, Blob, Timer) and scales automatically with load.
*proj: (direct) core compute for DIP's document processing pipeline (Service Bus-triggered functions).*

### Azure Blob Storage
Object storage for unstructured data (documents, images) at scale, organized in containers/blobs.
*proj: (direct) storing raw insurance documents in DIP, and migrating AWS storage-dependent integrations to Blob Storage.*

### Azure Files
Managed file shares over SMB/NFS — useful for lift-and-shift scenarios needing traditional file share behavior.

### Azure Service Bus
An enterprise message broker with queues and pub/sub topics, plus sessions, dead-lettering, and duplicate detection.
*proj: (direct) migrated DIP from Azure Storage Queues to Service Bus for topics/subscriptions and DLQ support.*

### Azure Storage Queues
A simpler, cheaper queue service for basic async messaging, without topics/subscriptions or advanced enterprise features.
*proj: (direct) original queueing mechanism in DIP before the Service Bus migration.*

### Azure Key Vault
A managed service for storing secrets, keys, and certificates securely, access controlled via RBAC/Managed Identity.
*proj: (direct) used for secrets in both DIP and the AWS→Azure modernization work.*

### Managed Identity
An Azure AD identity automatically given to a resource (like a Function App), letting it authenticate to other Azure services without stored credentials.
*proj: (direct) used to authenticate Azure Functions to Key Vault and Blob Storage in both DIP and the AWS→Azure modernization project.*

### RBAC
Role-Based Access Control — assigns permissions to an identity via roles scoped to a resource, resource group, or subscription.
*proj: (direct) scoped Managed Identity permissions (e.g., "Key Vault Secrets User", "Storage Blob Data Reader") to the minimum each service needed.*

### Azure AI Search
A managed search service supporting keyword, vector, and hybrid search with semantic ranking — the retrieval layer for RAG.
*proj: (direct) DIP (RAG retrieval) and Healthcare Staffing Platform (search across communication logs).*

### Azure OpenAI
Microsoft's managed offering of OpenAI models (GPT, embeddings) with enterprise security, regional deployment, and Azure AD integration.
*proj: (direct) core GenAI engine for document understanding/extraction in DIP and GreenShield.*

### Azure AI Document Intelligence
A service that pulls structured data (key-value pairs, tables, layout) out of documents like forms and scanned PDFs.
*proj: (direct) structured extraction from insurance claim documents in DIP.*

### Azure Cosmos DB
A globally distributed, multi-model NoSQL database with tunable consistency and very low latency at scale.
*proj: (direct) Healthcare Staffing Platform.*

### Azure SQL / SQL Server
Managed relational database service; strong for transactional, relational data with ACID guarantees.
*proj: (direct) Healthcare Staffing Platform (with Dapper).*

### Azure Monitor / Application Insights
Azure's observability stack — logs, metrics, and distributed traces for diagnosing performance and failures.
*proj: (direct) monitoring DIP's pipeline health and performance.*

### Virtual Network / Private Endpoint / Public vs Private Access
VNets isolate Azure resources on a private network; Private Endpoints let services like Key Vault/Storage be reached privately, skipping the public internet.
*proj: (would-apply) relevant to hardening the AWS→Azure modernization architecture so services talk privately instead of over public endpoints.*

### Autoscaling
Automatically adjusting instance count based on load metrics (CPU, queue length).
*proj: (direct) Azure Functions consumption plan scaling with Service Bus queue length in DIP.*

### Azure DevOps
Microsoft's suite for CI/CD pipelines, work item tracking, and repos.
*proj: (direct) used for build/release pipelines and reliable deployments across projects.*

### "Why did you migrate from Azure Storage Queues to Azure Service Bus?"
*A: "Storage Queues are simple and cheap but have no topics/subscriptions, so multiple business units couldn't independently consume the same message stream. We needed Service Bus's topics/subscriptions for parallel processing across business units, built-in dead-lettering for bad messages, larger message sizes, and stronger delivery guarantees — which improved both scalability and reliability."*

### "How does Azure Service Bus improve scalability and reliability?"
*A: "Topics/subscriptions let several independent consumers process the same events in parallel; built-in retry and dead-lettering isolate failures without blocking the main queue; and it handles higher throughput and bigger payloads than Storage Queues, so we can scale document processing horizontally while keeping failures contained and visible."*

### "What is Managed Identity?"
*A: "An Azure AD identity automatically given to a resource like a Function App, so it can authenticate to other Azure services (Key Vault, Blob Storage) using Azure AD tokens instead of connection strings or secrets — no credential to manage or leak."*

### "How does Managed Identity work with Azure Key Vault and other Azure resources?"
*A: "The resource (a Function App, say) gets a system- or user-assigned identity in Azure AD. We grant that identity an RBAC role (like 'Key Vault Secrets User') scoped to the specific Key Vault. At runtime, the SDK grabs an Azure AD token for that identity to call Key Vault — no secret ever sits in config."*

### "What is RBAC?"
*A: "Role-Based Access Control — assigning roles (each a set of permissions) to an identity, scoped at subscription, resource group, or resource level, following least-privilege."*

### "How would you secure communication between Azure services?"
*A: "Managed Identity + RBAC instead of shared secrets for service-to-service auth, Key Vault for any remaining secrets, private endpoints/VNet integration to avoid public internet exposure, and encryption in transit (TLS) and at rest by default."*

---

## SECTION 8: GENERATIVE AI, RAG AND AI ARCHITECTURE

### Generative AI
AI models that create new content (text, images, code) based on learned patterns, instead of just classifying/predicting.
*proj: (direct) Azure OpenAI-powered document understanding in DIP and GreenShield.*

### LLM
Large Language Model — a neural network (usually transformer-based) trained on huge amounts of text to understand and generate language.

### Tokens
The small text pieces LLMs process; cost and context limits are measured in tokens, not characters/words.

### Context Window
The max number of tokens (input + output) a model can handle in one call.
Drives your chunking strategy in RAG — retrieved context + prompt + query all need to fit within this limit.

### Prompt Engineering
Crafting inputs (instructions, examples, structure) to reliably steer an LLM toward the result you want.
*proj: (direct) designed extraction prompts for structured claim data in DIP.*

### Embeddings
Numeric vectors representing text meaning, so you can compare similarity.
*proj: (direct) generated embeddings for document chunks to enable semantic retrieval in DIP.*

### Vector Search
Finds the most similar items to a query by comparing embedding vectors (like cosine similarity) instead of exact keyword matches.
*proj: (direct) Azure AI Search vector search for RAG retrieval.*

### Semantic Search
Search based on meaning/intent (via embeddings) rather than exact keyword matches.

### Hybrid Search
Combines keyword (BM25) and vector search, sometimes with re-ranking, to get both exact-term precision and semantic recall.
*proj: (direct) used Azure AI Search's hybrid search for better retrieval quality in DIP than pure vector or pure keyword alone.*

### RAG (Retrieval-Augmented Generation)
Makes an LLM's answer better by retrieving relevant external content at query time and adding it to the prompt, instead of relying only on what the model already "knows."
*proj: (direct) core architecture pattern for DIP's document understanding.*

### Advanced RAG
Extras on top of basic RAG — hybrid search, re-ranking, query rewriting, metadata filtering, multi-step retrieval — to sharpen retrieval and grounding.
*proj: (direct) applied hybrid search and metadata filtering to improve retrieval relevance in DIP.*

### Chunking
Splitting large documents into smaller pieces before embedding, so retrieval returns focused, relevant content.

### Chunk Size
How long each chunk is (in tokens) — a trade-off between keeping enough context (larger) and precise retrieval/cost (smaller).
Start around 500–1000 tokens depending on document structure, then tune based on retrieval quality; structured forms may need smaller, field-aligned chunks, while narrative text can use larger ones.

### Chunk Overlap
A small overlap of tokens between chunks so context isn't lost at chunk boundaries.

### Metadata Filtering
Narrows retrieval results using structured metadata (document type, date, business unit) alongside semantic/keyword search.
*proj: (direct) filtering by document type/claim category in DIP's Azure AI Search queries.*

### Retrieval
The step in RAG where relevant chunks get pulled from the vector store/search index based on the user's query.

### Reranking
A second pass that reorders the initially retrieved results by finer-grained relevance, improving precision at the top.

### Grounding
Making sure the model's answer is based on the retrieved, factual context rather than its own possibly-stale internal knowledge.

### Hallucination
When an LLM makes up something that sounds plausible but isn't factually correct or supported.
Reduce it with strong grounding via RAG, explicit "answer only from context, otherwise say you don't know" instructions, citations, and lower temperature.

### Function Calling
LLM outputs a structured call to a defined function/tool (with arguments) instead of free text, so the app can execute real actions.
*proj: (direct) used function calling to trigger structured data extraction/downstream actions in DIP workflows.*

### AI Agents
Systems where an LLM plans and takes a series of actions (calling tools, reasoning over results) to reach a multi-step goal, instead of answering in one shot.
*proj: (direct) built agentic workflows using LangChain/LangGraph for document processing tasks.*

### Agentic Workflows
Multi-step, often stateful flows where an agent decides which tool to call next based on intermediate results, sometimes looping or branching.
*proj: (direct) LangGraph-based workflows for multi-step document understanding/extraction.*

### LangChain
A framework with building blocks (prompts, chains, retrievers, tool integrations) for building LLM apps.
*proj: (direct) used for RAG pipeline components and tool orchestration.*

### LangGraph
An extension of LangChain for building stateful, graph-based agent workflows with explicit control over branching, loops, and state.
*proj: (direct) used to build multi-step agentic document processing flows.*

### LLM Evaluation
Measuring an LLM/RAG system's output quality — relevance, faithfulness/groundedness, correctness — with metrics or LLM-as-judge.
Separate retrieval metrics (precision/recall of retrieved chunks) from generation metrics (faithfulness, relevance), using labeled test sets and/or LLM-as-judge scoring.

### AI Safety
Making sure AI systems behave reliably, ethically, and don't produce harmful or biased output.

### Guardrails
Rules (content filters, input/output checks, allowed topics) that keep an AI system's behavior within safe, expected bounds.
*proj: (direct) used Azure OpenAI's built-in content filtering and prompt-level constraints in enterprise document workflows.*

### "Explain the architecture of a RAG application you have worked on."
*A:* "For the Document Intelligence Platform, unstructured insurance documents are processed first — Azure AI Document Intelligence pulls out structured layout/text, and we chunk the text into overlapping segments sized to balance context and retrieval precision. Each chunk is embedded and stored in Azure AI Search with metadata like document type and claim ID. At query time, the user's question gets embedded, and we run hybrid search (keyword + vector) against Azure AI Search, optionally filtering by metadata, to pull the most relevant chunks. Those chunks go into a prompt sent to Azure OpenAI, which generates a grounded answer — we tell the model to answer only from the given context to cut down on hallucination. For more complex, multi-step tasks, we used LangGraph to run agentic workflows where the model could call more tools/retrieval steps before producing a final answer."

### How to reduce hallucination
Grounding via retrieved context, explicit "don't know if not in context" instructions, citations, lower temperature, evaluation-driven prompt tuning.

### How to improve retrieval quality
Hybrid search, better chunking, metadata filtering, reranking, query rewriting/expansion.

### How to choose chunk size
Balance context completeness vs. retrieval precision; match document structure; check the results empirically via retrieval evaluation.

### Vector vs Keyword vs Hybrid Search
Vector = semantic similarity (good for paraphrased/conceptual queries); Keyword = exact term matching (good for IDs/specific terms); Hybrid = both, usually best for enterprise documents mixing structured terms and narrative text.

### How to evaluate a RAG system
Separate retrieval quality (precision/recall @k) from generation quality (faithfulness, relevance) using labeled datasets and/or LLM-as-judge.

### Security considerations for enterprise RAG
Respect document-level access when retrieving, handle PII carefully, encrypt data at rest/in transit, avoid leaking sensitive data into prompts/logs, and use Managed Identity/Key Vault for credentials instead of hardcoded API keys.

---

## SECTION 9: REACT AND TYPESCRIPT

### Components / Props / State
Components are reusable UI pieces; Props are read-only inputs from parent to child; State is a component's own data that triggers a re-render when it changes.

### Functional Components / Hooks
Modern React components are plain functions using Hooks (`useState`, `useEffect`, etc.) for state and side effects, instead of class components.

### useState
A hook that adds local state to a functional component, returning the value and a setter that triggers re-render.

### useEffect
A hook for running side effects (data fetching, subscriptions, DOM work) after render, controlled by a dependency array.

### useMemo
Memoizes a computed value so it's only recalculated when its dependencies change, skipping expensive recomputation.

### useCallback
Memoizes a function reference so it doesn't get recreated every render — important when passing callbacks to memoized children.

### useRef
Holds a mutable value across renders without causing a re-render when it changes; often used for DOM references.

### Context API
Lets you share state across the component tree without passing props through every level ("prop drilling").

### Redux
A predictable global state container — changes happen via dispatched actions handled by pure reducer functions.
*use it when: many unrelated parts of a big app need to share/update state consistently, beyond what Context comfortably handles.*
*proj: (direct) Healthcare Staffing Platform frontend.*

### Redux-Saga
Middleware for handling side effects (API calls, async flows) in Redux using generator functions, giving fine control over async logic (cancellation, sequencing, retries).
*why: better than plain thunks for complex async flows, like coordinating multi-step notification delivery with retries/cancellation.*
*proj: (direct) Healthcare Staffing Platform's notification workflows.*

### Controlled vs Uncontrolled Components
Controlled components have their value driven by React state (`value` + `onChange`); uncontrolled components manage their own value internally, accessed via refs.

### Component Lifecycle
The phases a component goes through — mount, update, unmount — now handled via `useEffect` instead of lifecycle methods.

### Virtual DOM / Reconciliation / Keys
React keeps a lightweight in-memory copy (Virtual DOM), diffs it against the last version (reconciliation), and only updates the real DOM where needed; `key` props help React match list items correctly across re-renders.

### Rendering / Re-rendering
What causes a re-render: a state change, a prop change, a parent re-rendering (cascading down), or a context value changing.

### Performance Optimization / Memoization / React.memo
`React.memo` skips re-rendering a component if its props haven't changed (shallow compare); pair with `useMemo`/`useCallback` to keep prop references stable.

### Code Splitting / Lazy Loading
Splits the app bundle into smaller chunks loaded on demand (`React.lazy` + `Suspense`), cutting initial load time.

### Error Boundaries
Components that catch JS errors in their child tree and show a fallback UI instead of crashing the whole app.

### "What causes a React component to re-render?"
*A: State updates, prop changes, a parent re-rendering (unless memoized), or a subscribed context value changing.*

### "What is the difference between useMemo and useCallback?"
*A: `useMemo` memoizes a computed value; `useCallback` memoizes a function reference — `useCallback(fn, deps)` is basically `useMemo(() => fn, deps)`.*

### "When would you use Redux?"
*A: When many unrelated components across the app need to read/update shared state consistently, and Context alone would cause prop-drilling or unnecessary re-renders — like the Healthcare Staffing Platform's cross-cutting notification state.*

### "What is Redux-Saga and why would you use it?"
*A: Middleware using generator functions to manage complex async side effects (sequencing, cancellation, retries) more explicitly than thunks — useful for multi-step, real-time notification workflows.*

### "How do you optimize React application performance?"
*A: Memoize expensive computations/components (`useMemo`, `React.memo`), stabilize callback references (`useCallback`), use code splitting/lazy loading for big routes/components, virtualize long lists, and avoid unnecessary state lifting that triggers wide re-renders.*

---

## SECTION 10: DATABASES AND DATA

### SQL vs NoSQL
SQL databases (SQL Server, PostgreSQL) enforce a fixed schema with strong relational integrity/ACID transactions; NoSQL (Cosmos DB) offers flexible schemas and horizontal scale, sometimes trading off some consistency for availability/speed.

### Normalization / Denormalization
Normalization organizes data to cut redundancy (splitting into related tables); denormalization duplicates data on purpose to speed up reads, at the cost of extra write complexity.

### Indexes
Data structures that speed up lookups, at the cost of extra storage and slower writes.

### Clustered vs Non-Clustered Index
A clustered index sets the physical storage order of rows (one per table); a non-clustered index is a separate structure pointing back to the rows (you can have several).

### Composite Index
An index across multiple columns, useful when queries filter/sort by that combination together.

### Query Optimization / Execution Plans
Execution plans show how the DB engine will run a query (index seeks vs. scans, joins used) — reading them helps you spot missing indexes or slow patterns.

### Transactions / ACID
A transaction groups operations to succeed or fail as one unit, guaranteeing Atomicity, Consistency, Isolation, Durability.

### Isolation Levels
Control how much one transaction can see of another's uncommitted changes (Read Uncommitted → Read Committed → Repeatable Read → Serializable) — trading consistency for concurrency/speed.

### Deadlocks
Two transactions each hold a lock the other needs, so both wait forever until the DB engine detects it and kills one.

### Optimistic vs Pessimistic Concurrency
Optimistic assumes conflicts are rare — checks a version/timestamp at commit and fails if it changed; Pessimistic locks the data upfront to block any conflicting access.

### Pagination
Returning data in fixed-size pages (offset/limit or keyset-based) instead of the whole result set, for speed and UX.

### Connection Pooling
Reusing a pool of open DB connections across requests instead of opening/closing one each time, cutting latency/overhead.

### Caching
Covered in Section 3 — same idea applies to DB read-heavy patterns.

### "When should I use SQL versus Cosmos DB?"
*A: "I'd use SQL Server/PostgreSQL when data is relational, needs strong ACID transactions, and has a stable schema — like core claims/staffing records. I'd use Cosmos DB when I need global distribution, a flexible/evolving schema, very high write throughput, or single-digit-millisecond latency at scale with tunable consistency — which is why we used Cosmos DB alongside SQL Server in the Healthcare Staffing Platform for scenarios needing that flexibility and scale."*

---

## SECTION 11: SECURITY

### Authentication vs Authorization
Covered in Section 6.

### JWT / OAuth / OpenID Connect / RBAC / Managed Identity
Covered in Sections 6–7.

### Secrets Management / Azure Key Vault
Centralized, access-controlled storage for secrets/keys/certs instead of hardcoding them in config or code.
*proj: (direct) Key Vault across DIP and AWS→Azure modernization.*

### Encryption in Transit
Data is encrypted (TLS/HTTPS) while moving between client/server/services, so it can't be intercepted.

### Encryption at Rest
Data is encrypted while stored on disk (Azure Storage, SQL DB encrypt by default), protecting the underlying storage.

### HTTPS
HTTP over TLS — encrypts and authenticates communication between client and server.

### CORS
Covered in Section 6.

### SQL Injection
An attack where malicious input gets concatenated into a SQL query, changing its logic; prevented with parameterized queries/ORMs (EF Core, Dapper with parameters).

### XSS (Cross-Site Scripting)
An attack where malicious scripts get injected into pages other users view; React escapes JSX by default, but `dangerouslySetInnerHTML` and raw HTML need to be avoided/sanitized.

### CSRF
An attack that tricks an authenticated user's browser into making unwanted requests; mitigated via anti-forgery tokens or SameSite cookies.

### API Security
Combines authentication, authorization, input validation, rate limiting, and HTTPS to keep APIs safe from abuse and unauthorized access.

### Rate Limiting
Covered in Section 3/6.

### Least Privilege Principle
Give identities only the minimum permissions they need to do their job — nothing more.
*proj: (direct) scoped Managed Identity RBAC roles tightly (like specific Key Vault secret access, not broad Contributor roles).*

### "How would you secure an enterprise application deployed on Azure?"
*A:* "I'd layer security: JWT-based authentication with claims/role-based authorization at the API layer; Managed Identity + least-privilege RBAC for service-to-service auth instead of embedded secrets; Key Vault for any remaining secrets/certificates; HTTPS everywhere with TLS enforced; parameterized queries/ORMs to prevent SQL injection; input validation and React's default escaping (avoiding `dangerouslySetInnerHTML`) to prevent XSS; CORS locked to known frontend origins; rate limiting on public/AI-backed endpoints; and private endpoints/VNet integration so internal traffic doesn't traverse the public internet."

---

## SECTION 12: DEVOPS AND DEPLOYMENT

### CI/CD
Continuous Integration auto-builds/tests code on every commit; Continuous Deployment/Delivery automates releasing that code to environments.
*proj: (direct) Azure DevOps pipelines across projects.*

### Build Pipeline / Release Pipeline
Build pipeline compiles/tests/packages the app into an artifact; release pipeline deploys that artifact through environments (dev → staging → prod).

### Docker / Containers
Docker packages an app with its dependencies into a portable, isolated image that runs the same way everywhere.

### Containerization Benefits
Consistent environments (no more "works on my machine"), easier scaling/orchestration, faster deployment, isolation between services.

### Kubernetes Basics
An orchestrator that manages deployment, scaling, and healing of containerized apps across a cluster of nodes.

### Environment Configuration / Secrets Management
Externalizing config (connection strings, feature flags) per environment, with secrets pulled from Key Vault instead of checked into source control.

### Infrastructure as Code
Defining cloud infrastructure (ARM/Bicep/Terraform) as versioned code instead of manual portal clicks — repeatable and reviewable.

### Blue-Green Deployment
Two identical environments (blue = current, green = new); traffic switches to green after validation, so you can roll back instantly by switching back.

### Canary Deployment
Gradually rolling out a new version to a small slice of traffic/users first, watching for issues before a full rollout.

### Rolling Deployment
Replacing old instances with new ones a few at a time, keeping the app available the whole time.

### Monitoring / Logging
Covered in Section 3.

### SonarQube
Static code analysis tool that flags code smells, bugs, and vulnerabilities, and tracks code quality/coverage over time.
*proj: (direct) used at CDK Global to maintain application quality.*

---

## FINAL SECTION: TOP INTERVIEW QUESTIONS

**1. Tell me about yourself.**
"I'm a software engineer with 4+ years of experience, the last 2+ focused on Generative AI and Microsoft Azure. I've built AI-powered enterprise solutions using Azure OpenAI, RAG, and Azure AI Search — most recently on a Document Intelligence Platform that automates classification and extraction from unstructured insurance documents. I also have strong full-stack foundations in React/TypeScript and .NET/C#, having built a Healthcare Staffing Platform's notification workflows and enterprise apps with PostgreSQL/SQL Server. Currently, I'm working on an AWS-to-Azure cloud migration, modernizing enterprise integrations and Mirth Connect workflows to Azure Blob Storage, Service Bus, Key Vault, and Managed Identity. I enjoy combining GenAI with solid software engineering to build scalable, production-ready systems."

**2. Explain your current project's architecture.**
See AWS→Azure answer below — remediating Mirth Connect integrations, migrating AWS-dependent services to Azure Blob Storage/Service Bus/Key Vault/Managed Identity, using GitHub Copilot to accelerate analysis and remediation.

**3. Explain the Document Intelligence Platform architecture.**
See Section 1's detailed architecture answer (Blob Storage → Service Bus → Functions → Document Intelligence/OpenAI+RAG → Azure AI Search → DB).

**4. Explain your AWS-to-Azure migration work.**
"I'm working on modernizing an enterprise integration platform, migrating AWS-dependent Mirth Connect integrations and application components to Azure. This includes re-pointing storage-dependent connectors to Azure Blob Storage, migrating queue-based integrations to Azure Service Bus, moving secrets to Azure Key Vault, and replacing static credentials with Managed Identity. I also remediate Mirth Connect channels/scripts/connectors for Azure compatibility, troubleshoot connectivity/config issues, and use GitHub Copilot to accelerate large-scale code analysis and refactoring."

**5. How are you using Azure Service Bus?**
"As the messaging backbone for our document processing pipeline — Blob Storage upload events publish messages to Service Bus topics, which multiple Azure Functions subscribe to independently for classification, extraction, and indexing, enabling parallel processing across business units with built-in retry and dead-lettering for failure isolation."

**6. Why did you migrate from Azure Storage Queues to Azure Service Bus?**
See Section 7 answer.

**7. How are you handling authentication and authorization?**
See Section 6 answer.

**8. How do you secure Azure services?**
See Section 11 answer (Managed Identity, RBAC, Key Vault, private endpoints, encryption).

**9. Explain SOLID principles and where you have used them.**
See Section 2 summary answer.

**10. Explain Clean Architecture.**
See Section 1 — domain at the center, dependencies point inward, infrastructure/frameworks at the edge, enabling testability and framework independence.

**11. How does Dependency Injection work in .NET?**
"ASP.NET Core has a built-in DI container. You register services (interface → implementation) at startup with a lifetime — Singleton, Scoped, or Transient — and the framework injects the appropriate instance into constructors automatically, resolving the full dependency graph."

**12. How do you design a scalable system?**
See Section 3's document processing system design answer — stateless services, async messaging, horizontal autoscaling, caching, and failure handling.

**13. How do you handle failures in distributed systems?**
"Retry with exponential backoff for transient failures, circuit breakers to avoid hammering a failing dependency, dead-letter queues to isolate poison messages, idempotent consumers so retries are safe, and distributed tracing/correlation IDs to diagnose where failures occur."

**14. How do microservices communicate?**
See Section 4 answer — REST for sync, Service Bus for async.

**15. How do you maintain consistency in distributed systems?**
See Section 4 answer — eventual consistency, Saga pattern, idempotency.

**16. How do you improve API performance?**
"Async I/O throughout, caching frequent reads, pagination for large result sets, minimizing payload size, connection pooling for DB/HTTP clients, and avoiding N+1 query patterns via proper eager loading/projections."

**17. How do you optimize database performance?**
"Proper indexing (including composite indexes matching query patterns), analyzing execution plans to find scans vs. seeks, pagination, avoiding N+1 queries, and considering read replicas/partitioning for heavy read/write loads."

**18. How do you optimize React performance?**
See Section 9 answer.

**19. Explain RAG architecture.**
See Section 8 detailed answer.

**20. How do you reduce hallucinations in RAG?**
See Section 8 answer — grounding, explicit instructions, citations, lower temperature, evaluation.

**21. What is an AI Agent?**
"A system where an LLM doesn't just answer in one shot, but plans and executes a sequence of actions — calling tools/functions, reasoning over results, and deciding next steps — to accomplish a multi-step goal. I've built these using LangChain/LangGraph for multi-step document processing workflows."

**22. What is the difference between RAG and Fine-Tuning?**
"RAG augments the model with external, up-to-date context at query time without changing model weights — cheaper, faster to update, and grounds answers in your actual data. Fine-tuning changes the model's weights to bake in new behavior/style/knowledge, which is more expensive, slower to iterate, and better suited for teaching a consistent format/behavior rather than injecting frequently-changing factual data. In enterprise document use cases like ours, RAG is usually preferred since the underlying documents/knowledge change often."

**23. Explain a challenging technical problem you solved.**
"On the Document Intelligence Platform, our initial Storage Queue-based pipeline couldn't support multiple business units consuming the same document events independently, and failures in one document could stall processing. I led the migration to Azure Service Bus, using topics/subscriptions so each business unit's consumer processed events in parallel, and implemented dead-letter queue handling with idempotent, ID-based processing checks so retries never duplicated work. This significantly improved both scalability and reliability of the pipeline."

**24. Explain a system design decision and the trade-offs involved.**
"Choosing Service Bus over Storage Queues meant more cost and setup complexity, but the trade-off was justified by our real need for topics/subscriptions and stronger reliability guarantees. I also chose hybrid search over pure vector search for RAG retrieval — it added a bit of query complexity but significantly improved precision for queries containing exact terms like claim IDs alongside conceptual questions."

**25. Why should we hire you?**
"I bring a rare combination — solid full-stack engineering fundamentals (React, .NET, distributed systems, cloud migration) plus hands-on production experience building GenAI/RAG systems on Azure. I've not only built AI features but also owned the reliability and scalability of the systems delivering them, like migrating messaging infrastructure for better fault tolerance. I can operate across the full stack — from designing resilient cloud architecture to building the AI layer on top of it — which lets me contribute quickly without needing to be siloed into just "AI" or just "backend" work."

---

## FINAL 30-MINUTE REVISION CHEAT SHEET

*Architecture* — Microservices = independent scale/deploy, more ops complexity. Clean/Onion/Hexagonal = domain at center, dependencies point inward, testable & framework-independent. Event-driven = decoupled async via Service Bus.

*SOLID* — SRP=one reason to change. OCP=extend without modifying. LSP=subtypes substitutable. ISP=small focused interfaces. DIP=depend on abstractions, inject via DI.

*System Design* — Stateless + horizontal scaling + load balancing = scalability. Retry+Circuit Breaker+Bulkhead+DLQ = resilience. Idempotency = safe retries. CAP = pick 2 of Consistency/Availability/Partition tolerance (really C vs A trade-off).

*Microservices* — REST=sync/simple. Service Bus=async/decoupled. Saga=distributed transaction via compensating steps. Eventual consistency is the norm across services.

*.NET* — GC has 3 generations (Gen0 collected most). `using`/IDisposable for unmanaged resources. async/await = non-blocking I/O. IQueryable=DB-translated query; IEnumerable=in-memory. DI lifetimes: Singleton/Scoped/Transient — never inject Scoped into Singleton.

*ASP.NET Core* — JWT = stateless auth token. Claims/Policy/Role-based authorization. Middleware pipeline order matters. Idempotent HTTP methods: GET, PUT, DELETE.

*Azure* — Managed Identity = no stored secrets, RBAC-governed auth to Key Vault/Blob/etc. Service Bus > Storage Queues for topics/subscriptions + DLQ + reliability. Least privilege RBAC everywhere.

*GenAI/RAG* — RAG = retrieve relevant chunks (via Azure AI Search hybrid search) → inject into prompt → Azure OpenAI generates grounded answer. Chunking + overlap balance context vs. precision. Reduce hallucination = ground + instruct + cite + evaluate. Function calling/Agents (LangChain/LangGraph) = multi-step tool use. RAG vs Fine-tuning: RAG for frequently changing facts, fine-tuning for consistent behavior/style.

*React* — Re-render triggers = state/prop/parent/context change. useMemo=memoize value, useCallback=memoize function. Redux for large shared state; Redux-Saga for complex async side effects.

*Databases* — SQL = relational/ACID/strong consistency. Cosmos DB = flexible schema, global scale, tunable consistency. Index = faster reads, slower writes. Optimistic concurrency = check version at commit; pessimistic = lock upfront.

*Security* — Authn=who you are, Authz=what you can do. Managed Identity + Key Vault + least privilege + HTTPS + parameterized queries (no SQL injection) + React's default escaping (no XSS) + CORS locked to known origins.

*DevOps* — CI=build/test automatically, CD=automated release. IaC = versioned, repeatable infra. Blue-green = instant rollback via traffic switch; Canary = gradual rollout to subset first.

*My two headline stories to have ready:*
1. Service Bus migration in DIP (scalability + reliability + DLQ + idempotency).
2. AWS→Azure modernization (Managed Identity, Key Vault, Blob Storage, Mirth Connect remediation, GitHub Copilot-accelerated migration).

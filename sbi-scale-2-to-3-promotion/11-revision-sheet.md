# 11. Revision Sheet

Read this after the chapters. Each line is the sentence to be able to say without notes. Then use the short questions at the end.

## Front end

- Front end is the client side: what the customer sees and uses.
- HTML gives structure. CSS gives look and layout. JavaScript gives behaviour.
- A tag marks content. An element is the whole piece. An attribute adds information inside a tag.
- Semantic tags such as header, nav, main, and footer say what a region means.
- The CSS box model is content, padding, border, margin.
- The cascade decides which style wins. An id is more specific than a class. A class is more specific than a plain tag.
- Flexbox and grid are the modern layout tools. Media queries make a page responsive.
- The DOM is the live tree of the page. JavaScript changes the DOM when the customer clicks.
- Client-side checks are a convenience. The server must check again.
- JSON is the usual text format for data between a page and a server.
- React, Angular, and Vue are UI frameworks built from components. An SPA changes views without reloading the whole page.
- Java and JavaScript are different languages.

## Back end

- The back end holds rules, secrets, integrations, and data access.
- A web server receives HTTP. An application server runs business logic. A database stores records.
- Server-side processing means authenticate, authorise, validate, apply rules, update data, respond, and log.
- HTTP is stateless. Sessions and tokens add memory in a controlled way.
- Authentication asks who you are. Authorisation asks what you may do.
- MVC separates the request path, the business rules, and the presentation.
- ACID: atomicity, consistency, isolation, durability. Debit and credit must succeed together or not at all.
- An idempotent payment can be retried without a second debit.

## Architecture

- Client asks. Server answers. Customers do not connect directly to the database.
- 3-tier means presentation, application, and data.
- 2-tier often lets the client reach the data directly. That is a weaker shape for internet banking.
- A monolith is one deployable application containing many functions.
- Microservices split capabilities so each can be deployed and scaled more independently.
- SOA is the older idea of reusable enterprise services, often with a central bus.
- A load balancer spreads traffic. A DMZ buffers the inner network from the internet.

## Security

- Confidentiality, integrity, availability.
- OWASP publishes the Top 10 web application risks. It is guidance, not a statute.
- Broken access control: the server did not enforce permission. Hiding a button is not a control.
- SQL injection is stopped by parameterised queries.
- XSS is a script injected into a page other people view. CSRF is a forged request that rides the victim's cookie.
- HTTPS is HTTP over TLS. SSL is the retired ancestor.
- A certificate binds a site name to a public key and is signed by a trusted CA.
- A secure session uses a random id, regenerated at login, stored as data on the server, and sent in a Secure, HttpOnly, SameSite cookie.
- Logout must destroy the session on the server.

## APIs

- An API is a programming interface. A web service offers that kind of interface over the network.
- REST is a style: resources, HTTP methods, usually JSON, self-contained requests.
- SOAP is a protocol: an XML envelope, described by WSDL.
- GET reads and must not move money. POST is used to submit a new action.
- 401 means not authenticated. 403 means authenticated and not allowed.
- An idempotency key stops a retried payment from posting twice.
- The API gateway authenticates, routes, rate-limits, and logs. The service still checks that this caller owns this account.
- OAuth lets a customer grant limited access without giving away the banking password.

## Performance and scalability

- Performance is speed now. Scalability is behaviour as load grows.
- Watch percentile latency, throughput, errors, and saturation. Fix the bottleneck you measured.
- Minify shortens code. Compression packs it for travel. A CDN serves shared static files from nearby.
- Cache shared, slow-changing data. Do not debit from a stale cached balance.
- Scale up means a bigger machine. Scale out means more machines.
- Queues take slow side work off the customer's wait.
- Timeout, circuit breaker, bulkhead, and careful retry stop one failure from becoming a collapse.

## Hosting and cloud

- IaaS is infrastructure. PaaS is a platform for your application. SaaS is a finished application.
- Public cloud is shared. Private cloud is for one organisation. Hybrid mixes private or on-premises with public.
- The provider secures the cloud. The bank secures identity, configuration, application, and data.
- Releases move through development, test, UAT, and production.
- RPO is how much data you can afford to lose. RTO is how fast you must be back.
- Serverless means you do not manage the server. A server still exists.

## Generative AI

- AI is the field. Machine learning learns from data. Deep learning uses deep neural networks. Generative AI creates content.
- Supervised learning needs labels. Unsupervised learning finds structure. Reinforcement learning uses reward.
- An LLM generates likely language. It does not look up facts the way a ledger does.
- A hallucination is a fluent false statement.
- RAG searches approved documents and then writes from those passages.
- A prompt should set the role, the task, the allowed source, the limits, and the output shape.
- Prompt injection hides hostile instructions inside data.
- In a bank, the model may draft, summarise, translate, and extract. It does not own the rate, the ledger, or the final decision.
- Governance needs an owner, least data, testing, logging, monitoring, and a way to switch the tool off.

## Big data

- 5Vs: volume, velocity, variety, veracity, value.
- Warehouse: structured, for trusted reports. Lake: raw variety. Swamp: an ungoverned lake.
- HDFS stores. MapReduce and Spark process. YARN manages cluster resources. Kafka moves events. Hive queries data with SQL-like language.
- OLTP runs live transactions. OLAP analyses them.
- Descriptive, diagnostic, predictive, prescriptive.
- Eventual consistency can suit a catalogue. A debit needs a real transaction.
- Governance names an owner, defines data, measures quality, traces lineage, and limits use.

## Other emerging technologies

- Blockchain is a hash-linked shared ledger agreed by consensus. Banks look at permissioned chains. A smart contract is code.
- IoT is connected sensing. Every device needs identity, patching, and a tight network path.
- Cloud-native combines containers, Kubernetes, automation, and observability.
- A container is lighter than a virtual machine because it shares the host operating system.
- Edge computing handles urgent local work. The centre keeps the ledger and the wide view.
- RPA is a rules bot for repetitive digital steps, with its own identity and a human exception queue.
- Quantum computers use qubits. A future large machine threatens RSA and elliptic-curve cryptography. The present action is post-quantum cryptography and the ability to change algorithms.

---

## Short questions

Answer in a sentence before you look at the line below the question.

**1. Why is client-side validation not security?**
The customer can bypass the browser and talk to the server directly, so the server must repeat every important check.

**2. What sits between the browser and the database in 3-tier architecture?**
The application tier, which applies business rules.

**3. Which OWASP idea covers "I changed the account number in the request and saw someone else's statement"?**
Broken access control.

**4. What is the difference between 401 and 403?**
401 means the caller did not prove identity. 403 means the caller is known and is not permitted.

**5. Why must a fund transfer not be a GET?**
GET is for safe reads and may be repeated by browsers and links. A transfer changes data and should be a POST protected by an idempotency key.

**6. What do HttpOnly, Secure, and SameSite do?**
HttpOnly hides the cookie from JavaScript. Secure sends it only over HTTPS. SameSite limits cross-site sending, which reduces CSRF.

**7. What does TLS give that a login does not?**
A protected channel and proof of the server certificate. Login proves the customer. They solve different problems.

**8. Why can an LLM invent a circular number?**
It produces likely text. It is not a system of record. Ground it with RAG and require a citation.

**9. Name the 5Vs.**
Volume, velocity, variety, veracity, value.

**10. Which Hadoop piece stores, which processes, and which assigns resources?**
HDFS stores. MapReduce or Spark processes. YARN assigns resources.

**11. IaaS, PaaS, SaaS?**
You rent machines. You rent a platform and bring the application. You rent the finished software.

**12. Why is a public blockchain a poor default for customer KYC documents?**
The data would be widely replicated and hard to delete. A bank design keeps private data off-chain and uses a permissioned ledger only if sharing is lawful and useful.

**13. How is RPA different from an LLM?**
RPA repeats explicit rules on systems. An LLM generates language and can be wrong. They can be joined, and they are not the same tool.

**14. What should the bank do about quantum computing this year?**
Inventory public-key cryptography, protect long-lived secrets, and plan a move to standardised post-quantum algorithms. Do not expect a quantum replacement of core banking.

**15. What is the shared responsibility model?**
The cloud provider secures the underlying cloud. The bank secures how it configures, accesses, builds, and uses the service, and it remains accountable for customer data.

## A last-night checklist

- Can you tell HTML, CSS, and JavaScript apart with one example each?
- Can you draw browser, web server, application, database?
- Can you define authentication and authorisation without using one to mean the other?
- Can you name five OWASP risks in plain language?
- Can you explain HTTPS, TLS, and a session cookie?
- Can you compare REST and SOAP in four lines?
- Can you say what an API gateway adds?
- Can you give one scale-out example and one cache warning?
- Can you place IaaS, PaaS, and SaaS?
- Can you tell AI, ML, LLM, RAG, and hallucination apart?
- Can you recite the 5Vs and four analytics types?
- Can you give one safe bank use and one risk for blockchain, IoT, edge, RPA, and quantum?

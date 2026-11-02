# 3. Web Architecture

## The one story

Architecture is the building plan. It answers three questions before anyone paints a screen:

1. Which computers do which jobs?
2. How do they talk?
3. What happens when one part becomes busy, fails, or must change?

A branch building has a customer hall, a processing area, and a strong room. A web application has a similar separation. Customers stay in the hall. They do not walk into the strong room. Staff carry the request inward through a controlled path.

**Say it in one line:** Web architecture is the plan of parts and conversations that turns a tap on a phone into a safe change in the bank's books.

---

## Client-server architecture

### The two roles

- The **client** asks. It is the browser, the mobile app, or sometimes another system.
- The **server** answers. It holds the service and, usually, the data.

One server can serve many clients. The clients do not need a copy of the bank's full ledger. They need a way to ask.

This pattern is old and still the base of the web. Email works as client-server. Core banking terminals work as client-server. A browser talking to an internet-banking system is client-server.

### What "thin client" and "thick client" mean

| Kind | Simple meaning | Example |
| --- | --- | --- |
| Thin client | Most logic stays on the server. The client mainly displays and collects input | A browser page that posts a form and shows the server's HTML |
| Thick client | The client contains a lot of logic and may store data locally | A full mobile app that does rich work on the device and calls the server for ledger actions |

Modern web front ends are in between. The screen logic is rich, and the money logic stays on the server. For the exam, the safe sentence is: presentation can be rich on the client, control stays on the server.

### A request's journey in a client-server web app

1. The customer types a web address or taps a button.
2. The device asks DNS for the numeric address of the bank's site. DNS is the internet's phone book.
3. The device opens a connection, usually a secure one, to the bank's server.
4. It sends an HTTP request.
5. The server processes it and sends an HTTP response.
6. The browser shows the result.

Client-server does not, by itself, say how many layers sit inside the server. That is the next idea.

### Strengths and strains

Strengths:

- Data stays central, so there is one ledger, not a different balance on every phone.
- Updates to rules can be deployed on the server.
- Clients can be many and cheap.

Strains:

- If the server is down, clients cannot complete the work.
- If all logic and data sit in one pile, growth and change become hard. Layers and microservices are answers to that strain.

---

## Tiers and layers

People say **tier** and **layer** as if they were always the same. Be precise enough for an exam.

- A **layer** is a logical separation inside the design: presentation, business rules, data. They might still run on one machine.
- A **tier** is a physical separation: those responsibilities run on different machines or different groups of machines.

In many exam questions the intended answer treats 3-tier as presentation, application, and database on separate tiers. Use that meaning unless the question carefully distinguishes layer from tier.

### 1-tier

Everything is in one place: screen, rules, and data. An old desktop program with a local file is 1-tier. It does not suit a bank's shared ledger.

### 2-tier

Two parts. Usually the client talks directly to the database, and a good deal of logic sits in the client.

Picture: each branch PC connects straight to the central database.

The strain: every client needs a database path, business rules get copied into many clients, and it is harder to protect the database. This pattern is generally a poor fit for internet banking.

### 3-tier

Three responsibilities, separated.

| Tier | Also called | What lives here | Bank picture |
| --- | --- | --- | --- |
| Presentation tier | Client tier, UI tier | Browser or mobile app. Screens, input, basic display checks | The customer's phone |
| Application tier | Business tier, logic tier, middle tier | Rules, validations, calculations, calls to other systems | The internet-banking application servers |
| Data tier | Database tier | The database and the durable records | The core banking database |

The customer never opens a connection to the database. The application tier is the only normal door.

Why banks like 3-tier:

- The database can sit in a stricter network zone.
- Application servers can be added when festival-day traffic grows, without giving customers database passwords.
- Teams can change a screen and a rule on different release cycles.
- Security controls can be placed at the middle tier: login, limits, and audit.

A standard sentence: **In 3-tier architecture, the client handles presentation, the application server handles business logic, and the database server handles data storage.**

### N-tier

**N-tier** means more than three physical separations when the system needs them. For example:

1. Browser.
2. Web server.
3. Application server.
4. A separate integration service.
5. Database.

It is 3-tier thinking extended. More tiers can improve separation. Too many tiers can add delay and operational burden. Architecture is a balance.

### Where the web server sits

In many real deployments the presentation tier is only the browser, and a web server sits with the application side:

`Browser → Web server → Application server → Database`

Some textbooks draw the web server inside the presentation tier because it produces HTML. Follow the question's diagram. The principle to defend is the same: customers do not connect directly to the database.

---

## Monolith

Before microservices, meet the thing they are compared with.

A **monolith** is one deployable application that contains many functions together: login, accounts, transfers, statements, and complaints in one program and usually one database.

The good side:

- Simple to develop at the beginning.
- One deployment, one log location, easy local testing.
- Transactions inside one database are straightforward.

The strain, once the system is huge:

- A small change in complaints can require rebuilding and retesting the whole application.
- A traffic spike in balance enquiry forces you to scale everything, including parts that are quiet.
- A fault can pull down unrelated journeys.
- Many teams editing one codebase get in each other's way.

Plenty of stable bank systems are monoliths. "Monolith" is not an insult. It is a shape with a cost that grows as the system grows.

---

## Microservices

### The idea

A **microservice architecture** splits the application into small services. Each service owns one business capability, runs as its own process, and communicates through the network, usually by APIs or messages.

Possible split for a digital bank channel:

| Service | Owns |
| --- | --- |
| Customer profile service | Name, contact, KYC status |
| Account service | Account list and balances for the channel |
| Payments service | Transfer requests and their status |
| Beneficiary service | Saved payees and cooling rules |
| Notification service | SMS, email, and app alerts |
| Authentication service | Login, OTP, and sessions |

Each can be deployed, restarted, and scaled on its own. If balance enquiry is busy on salary day, you add copies of the account read path without deploying a new payments service.

### How a request crosses services

The customer's app usually talks to one front door, often an **API gateway**. The gateway routes "transfer" to the payments service. The payments service may call the account service and the fraud service. The customer does not juggle five addresses.

### Data ownership

A strict microservice design gives each service its own data store. The payments service does not sneak into the profile service's tables. It asks the profile service through an API. This avoids hidden coupling.

The cost: a business action that used to be one database transaction may now span services. You then need careful design: retries that do not double-pay, compensating actions, and clear status such as "pending", "successful", and "reversed".

### Words that travel with microservices

| Word | Simple meaning |
| --- | --- |
| Independently deployable | You can release one service without releasing all the others |
| Loose coupling | Services know as little as possible about each other's insides. They depend on contracts |
| Bounded context | A service has a clear edge. "Beneficiary" does not slowly absorb "payments" |
| API contract | The agreed request and response. Callers rely on it |
| Stateless service | The service does not keep a customer's session in its own memory, so any copy of the service can handle the next request |
| Service discovery | A way for services to find each other's current address, because addresses change as copies come and go |
| Orchestration | One coordinator directs the steps of a journey |
| Choreography | Each service reacts to events without one central conductor |
| Synchronous call | The caller waits for the answer |
| Asynchronous message | The caller sends a message and continues. The other service handles it when ready |
| Fault isolation | A failure in notifications should not, by itself, erase a transfer that already succeeded. The alert can be retried |
| Observability | Logs, metrics, and traces that let you follow one customer request across many services |

**Distributed tracing** gives one request a trace id and passes it to every service, so support staff can see the whole path of a failed payment.

### Benefits and costs, said honestly

Benefits:

- Teams can own a service each.
- Scaling matches real demand.
- A fault can be contained.
- Technology can vary by service when there is a real reason.

Costs:

- Network calls can fail and add delay.
- Operations become harder: many deployments, many logs, many health checks.
- Data consistency across services needs design.
- Testing one customer journey means testing several services together.
- Without discipline, you get a **distributed monolith**: many services that must still be released together because they are secretly tied.

A senior manager's judgement: choose microservices when independent change and scale are worth the operational cost. A small, stable application can remain a well-built monolith or a modular application.

---

## Service-oriented architecture, because questions still use the name

**SOA**, service-oriented architecture, is the older enterprise idea of building systems as reusable services. An **enterprise service bus (ESB)** often sat in the middle, transforming messages and routing them between core banking, cards, loans, and channels.

| Idea | SOA with a central bus | Microservices |
| --- | --- | --- |
| Goal | Share and integrate enterprise functions | Small services that can change and scale independently |
| Communication | Often the bus, sometimes SOAP | Often direct REST or lightweight messages |
| Data | Frequently shared enterprise data | Preferably owned by each service |
| Deployment | Services could still be large | Emphasises independent deployment |

SOA is an ancestor, not a dirty word. Many banks still run ESB integrations beside newer microservices. If a question says "reusable services across the enterprise", SOA language fits. If it says "independently deployable small services", microservices language fits.

---

## Other architecture words you will meet beside the syllabus line

### Load balancer

A **load balancer** sits in front of several copies of a server and spreads requests. If one copy dies, the balancer stops sending people there.

Picture: one token counter becomes five counters, and a floor manager sends the next customer to a free counter.

### Cache

A **cache** keeps a recently fetched answer close by, so the system does not recompute or refetch it every time. Branch address lists and today's foreign-exchange card rates may be cached for a short time. A customer's available balance for a debit is a poor thing to cache for long, because it changes when a transaction is posted.

### Failover and high availability

**High availability** means the service keeps working even when a part fails.

**Failover** means a standby takes over. An active database and a standby database in another hall of the data centre are a common bank pattern.

**Redundancy** means you deliberately have more than one of a critical part.

### Horizontal and vertical growth

These are explained fully in the performance chapter. In architecture language:

- **Vertical scaling** means a bigger machine.
- **Horizontal scaling** means more machines, which is why stateless application tiers and load balancers matter.

### Network zones

A bank does not put every server on one flat network.

A simple picture:

| Zone | Who can reach it | What sits there |
| --- | --- | --- |
| Internet / external | Customers, through controlled doors | The public entry, after firewalls |
| DMZ | Only what must face the outside | Web servers, API gateways |
| Application zone | Only the DMZ and approved internal callers | Application services |
| Data zone | Only application services that need it | Databases |
| Management zone | Administrators through strong controls | Monitoring and admin tools |

**DMZ** means demilitarised zone. It is a buffer between the public internet and the inner network. A web server can live there. A core database should not.

**Firewall** means a control that allows or blocks traffic by rule. Security groups and similar cloud controls are the same idea in a newer form.

### Synchronous core and channel

In many banks the mobile channel is a separate system from **core banking**. The channel asks the core to post a transaction. The core remains the system of record for the ledger. Architecture questions sometimes test this: the channel is not the ledger. The core is.

---

## Three pictures side by side

| Question | Client-server | 3-tier | Microservices |
| --- | --- | --- | --- |
| What is it mainly about? | Who asks and who answers | Separating presentation, logic, and data | Splitting logic into independently deployable services |
| How many roles? | At least two | Three tiers | Many small services behind a front door |
| Can they combine? | Yes. A microservice system is still client-server, and it still keeps data away from the browser | Yes. Each microservice can have its own logic and data, while the whole channel still has presentation, logic, and data | Yes |

A strong exam sentence: **Microservices do not replace client-server. The mobile app is still the client. Microservices are a way of shaping the server side. A 3-tier split remains visible: the app presents, services decide, and databases store.**

---

## A salary-day story

At 6 pm on the last working day, lakhs of customers open the app.

- Client-server: each phone asks, the bank's servers answer.
- 3-tier: phones do not touch the database. Application servers do.
- Load balancer: requests spread across many application servers.
- Microservices, if used: the account-read service scales up. The complaint service stays quiet.
- Data tier: the core ledger remains the source of truth and stays protected in the inner zone.
- If one application server fails, the load balancer uses the others. That is failover in action.

---

## Architecture mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "In 3-tier, the browser connects to the database" | The application tier stands between them |
| "3-tier and 3-layer must mean three machines" | Tier leans physical. Layer leans logical. Exams often want the three-role answer |
| "Microservices means there is no database" | Each service still needs storage. The storage is usually separate per service |
| "A monolith cannot be client-server" | A monolith is one server-side application. Clients still call it |
| "More tiers are always better" | Each extra hop adds delay and moving parts |
| "The API gateway replaces the services" | It is the front door. The rooms are still behind it |

## Remember these lines

- Client asks, server answers, data stays central.
- 2-tier often means client talking directly to data. 3-tier puts an application tier between presentation and data.
- N-tier adds further physical separation when needed.
- A monolith packages many functions as one deployable system.
- Microservices split capabilities so they can be deployed, scaled, and failed over more independently.
- SOA is the broader enterprise-service idea. An ESB was its common middle.
- Load balancers, caches, zones, and failover are how the plan survives real traffic and real failures.

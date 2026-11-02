# 5. APIs and Web Services

## The one story

The mobile app does not contain the ledger. The SMS system does not contain the ledger. The ATM switch does not contain the customer-care complaint history.

Each system is good at its own job. When one needs help from another, it does not reach into the other's files. It walks up to a service window and makes a formal request.

That service window is an **API**. When the window is offered over a network, in a standard way, people often call the arrangement a **web service**.

Two common styles of window are **REST** and **SOAP**. A bank with many windows puts an **API gateway** in front as the reception desk.

**Say it in one line:** An API is a controlled way for one program to ask another program to do something. REST and SOAP are two styles of that conversation. The API gateway is the guarded front door.

---

## API

### What the letters mean

**API** means Application Programming Interface.

Break the phrase:

- **Application** — a program.
- **Programming** — used by programs, not only by humans clicking a screen.
- **Interface** — the agreed surface. The caller learns the surface and does not need the private machinery behind it.

A bank counter is an interface for humans. An API is an interface for software.

### A human example that maps cleanly

To ask for a cheque book you do not enter the stationery room. You give the counter a request in an expected shape: account number and number of leaves. The counter gives back a result: accepted, with a reference, or rejected, with a reason.

An API does the same:

- A defined address.
- A defined input.
- A defined output.
- A defined set of errors.

### Words around an API

| Word | Simple meaning |
| --- | --- |
| Endpoint | One specific address and action, such as "get account balance" |
| Request | What the caller sends |
| Response | What the provider sends back |
| Contract | The published promise of inputs, outputs, and errors. Callers build against the contract |
| Consumer or client | The program that calls |
| Provider | The program that serves |
| Version | A generation of the contract, such as v1 and v2, so old callers do not break when the API improves |
| Public API | Exposed to outside parties, such as a fintech the customer has permitted |
| Private or internal API | Used only inside the bank, such as the mobile channel calling the payments service |
| Partner API | Shared with a known outside party under an agreement |

### API and web service

In loose speech the words overlap. For an exam, this distinction is enough:

- **API** is the general word for a programming interface. It might be a web API, a library of functions inside one program, or an operating-system call.
- **Web service** is a service offered over the network using web standards, so another system on another machine can call it. SOAP services are web services. RESTful HTTP APIs are the modern web style and are often called web APIs. Older textbooks say "web service" mainly for SOAP and "REST API" for REST.

If a question treats REST and SOAP as two kinds of web services, accept that framing. It is common in bank syllabi.

---

## The language both sides must share

### HTTP methods

REST, and most modern web APIs, ride on HTTP. The method says what kind of action this is.

| Method | Intent | Bank example |
| --- | --- | --- |
| GET | Read. Should not change data | Fetch a mini statement |
| POST | Create, or start an action | Submit a new fund transfer |
| PUT | Replace a whole resource | Replace a full beneficiary record |
| PATCH | Change part of a resource | Change only the mobile number |
| DELETE | Remove | Delete a saved beneficiary, if the business allows it |
| HEAD | Like GET, but only headers | A health or cache check |
| OPTIONS | Ask what methods are allowed | Used by browsers in some cross-site checks |

Safe memory:

- GET is for reading.
- POST is the usual way to submit a new transaction.
- GET must not transfer money. A link, a retry, or a browser preload might call a GET. You do not want that to debit an account.

### Idempotent, said again because APIs depend on it

An operation is **idempotent** if doing it once and doing it several times with the same intent leaves the same result.

- GET is idempotent. Reading the balance twice does not change it.
- PUT and DELETE are designed to be idempotent.
- POST is not automatically idempotent. Submitting "pay ₹ 500" twice might pay twice unless the API uses a unique request id and recognises the retry.

Payment APIs should accept an idempotency key: a unique id the caller generates for that payment. If the network times out and the caller retries with the same key, the bank returns the original result instead of posting a second debit.

### Status codes

The number at the top of the response is a shared code.

| Code | Meaning | When a bank API uses it |
| --- | --- | --- |
| 200 | OK | The read succeeded |
| 201 | Created | A beneficiary or a transfer request was created |
| 202 | Accepted | The work is received and will finish later. Useful for payments that settle asynchronously |
| 204 | No content | Success, and there is nothing to show |
| 400 | Bad request | The amount is missing or malformed |
| 401 | Unauthorized | The caller did not prove identity. The name is historical. Think "unauthenticated" |
| 403 | Forbidden | The caller is known and is not allowed. Think "unauthorised" |
| 404 | Not found | No such resource, or you must not reveal that it exists |
| 409 | Conflict | The request clashes with current state, such as a duplicate reference |
| 422 | Unprocessable | The shape is fine and the business rule failed, in APIs that use this code |
| 429 | Too many requests | Rate limit exceeded |
| 500 | Internal server error | The server failed unexpectedly |
| 502 or 503 or 504 | Bad gateway, unavailable, or timeout | A downstream system or the service itself is not healthy |

401 and 403 are a favourite pair. 401 means "I do not know you, or your credential failed". 403 means "I know you, and the answer is no".

### Headers

**Headers** are metadata around the body. Examples: content type, authorisation token, a correlation id for tracing, and the idempotency key. They are like the notes on a form: who sent it, how to read the attachment, and the file reference.

### JSON and XML

| Format | Looks like | Where it shines |
| --- | --- | --- |
| JSON | `{ "amount": 500, "currency": "INR" }` | REST APIs and mobile apps. Light and easy |
| XML | `<amount currency="INR">500</amount>` | SOAP messages and many older enterprise interfaces. Verbose and strict |

Both are text. Both can be logged, so neither should carry a password "because it is only text".

---

## REST

### What the letters mean

**REST** means Representational State Transfer. It is an architectural style, described by Roy Fielding. It is not a product and not a single official standard document in the way SOAP has a stack of standards.

A **RESTful API** is an API that follows REST ideas closely. In daily work, people say REST when they mean "HTTP plus resources plus JSON".

### The central idea: resources

A **resource** is a thing the API exposes. It has a name, which is a URL.

Examples:

- `/accounts/12345678901` is one account.
- `/accounts/12345678901/transactions` is that account's transactions.
- `/beneficiaries/88` is one saved payee.

The URL names the thing. The HTTP method says what to do with the thing. You usually do not put the verb in the URL.

A clear style: `POST /transfers` with a body that contains from-account, to-account, and amount.

A weaker style: `POST /doTransferNow` with the action buried in a made-up sentence. It works, and it is less RESTful.

### Six constraints, in human language

Textbooks list these. Learn the meaning, not as a chant only.

| Constraint | What it asks for | Bank meaning |
| --- | --- | --- |
| Client-server | The caller and the provider are separate | The app does not contain the ledger |
| Stateless | Each request carries what the server needs to handle it. The server does not depend on hidden memory of earlier requests | The token or the data needed for this call is in the call. Any healthy server copy can handle it |
| Cacheable | Responses say whether they may be stored and reused | A list of branch timings can be cached. A debit response must not be casually reused |
| Uniform interface | Resources, standard methods, and self-describing messages | GET means read across the bank's APIs, not "GET sometimes transfers" |
| Layered system | The caller need not know whether a gateway, a cache, or the final service answered | The app talks to the gateway and does not know how many internal hops follow |
| Code on demand | Optional. The server may send code for the client to run | Rare in banking APIs. You can mention it as optional |

Stateless deserves care. The bank still remembers the customer, in a session store or inside a token. "Stateless" means that memory is not trapped inside one particular server's private variables. The next request can land on another server.

### Representations

The resource is the account. The **representation** is the JSON picture of the account you send over the wire. The same account might be represented as JSON to a mobile app and as a PDF statement to a customer. REST separates the thing from the picture of the thing. That is the "representational" part of the name.

### Richardson maturity, only if you want a deeper line

A simple ladder some architects mention:

1. One URL, and everything is a POST. This is barely REST.
2. Many resource URLs.
3. Use of HTTP methods properly.
4. **HATEOAS**, where responses contain links to the next legal actions.

HATEOAS means Hypermedia As The Engine Of Application State. The response tells the client what it can do next, through links. Full HATEOAS is uncommon in bank APIs. Knowing the word is enough.

### Why banks like REST for new channels

- Mobile apps and browsers speak HTTP naturally.
- JSON is light for phones.
- Tools, gateways, and developers are plentiful.
- It fits microservices.

REST does not magically give security, reliability, or a good design. A careless REST API can still expose all accounts or post a transfer twice.

**Say it in one line:** REST treats bank things as resources with URLs, uses HTTP methods for actions, prefers JSON, and keeps each request self-contained so any server copy can answer it.

---

## SOAP

### What the letters mean

**SOAP** means Simple Object Access Protocol. The expansion is historical. Modern SOAP is a standard way to send structured XML messages, and it is not especially simple.

A SOAP message is an **envelope**.

| Part | What it holds |
| --- | --- |
| Envelope | The outer wrapper that says "this is a SOAP message" |
| Header | Optional control information: identity, a signature, a transaction id, routing |
| Body | The actual request or response, such as the payment instruction |
| Fault | The error, in a defined SOAP shape, when the call fails |

### The papers that travel with SOAP

| Standard | Role |
| --- | --- |
| WSDL | Web Services Description Language. An XML document that describes the operations, inputs, and outputs. It is the formal contract. Tools can generate client code from it |
| XML Schema | The strict grammar of the data: which fields exist, which are mandatory, what types they have |
| UDDI | Universal Description, Discovery, and Integration. An old directory idea for finding services. Rare in new work. Mention it only as the historical third piece beside SOAP and WSDL |
| WS-Security | A standard way to sign and encrypt SOAP messages and to pass identity inside the message |
| WS-Addressing, WS-ReliableMessaging | Further standards for routing and reliable delivery |

This stack is why people say SOAP is heavy and formal. The formality is also why a core-banking or government interface may still choose it: strict contracts, message-level security, and operations that do not have to map neatly onto "resources".

### A SOAP call, as a story

The channel builds an XML envelope: "Debit this account, credit that beneficiary, amount, reference, signature". It posts that envelope to the service address. The receiver checks the schema and the signature, posts the transaction, and returns an envelope with a response code and a host reference. If it fails, the envelope carries a fault.

The transport is often HTTP, but SOAP was designed so the message could travel over other transports too. In practice you will meet SOAP over HTTP.

### REST and SOAP beside each other

| Question | REST | SOAP |
| --- | --- | --- |
| What is it? | An architectural style | A protocol with a strict message format |
| Typical payload | JSON | XML envelope |
| Contract | Often an OpenAPI document, or the team's published examples | WSDL plus schema |
| Action style | HTTP method plus resource URL | An operation inside the message, such as `MakePayment` |
| Formal standards for security and reliability | Use HTTPS, OAuth, and API design practice. Fewer built-in WS-* standards | WS-Security and related standards are part of the tradition |
| Feel | Lighter, common for mobile and microservices | Heavier, common for legacy enterprise integration |
| Error style | HTTP status plus a JSON error body | SOAP fault |

A mature sentence for a senior manager: new customer channels are usually REST. Some core, card, and inter-institution interfaces remain SOAP because they already have strict XML contracts and message security. The bank often translates between them at an integration layer.

**OpenAPI**, formerly associated with Swagger, is a popular way to describe a REST API in a file that humans and tools can read. It plays a role for REST similar to the role WSDL plays for SOAP. They are not the same format.

**Say it in one line:** SOAP sends a strict XML envelope described by a WSDL. It is formal and still present in enterprise banking. REST is the lighter resource style used by most new web and mobile APIs.

---

## API gateway

### What it is

An **API gateway** is a front door that sits between callers and many back-end services.

The mobile app knows one address. The gateway:

- Checks who is calling.
- Decides which service should receive this request.
- Applies limits.
- Records the call.
- Sometimes reshapes the request or the response.

It is a reverse proxy with API duties. In a microservices estate it is almost always present. In a smaller system a single application may serve clients directly, and a separate gateway appears when the number of services and clients grows.

### What a gateway typically does

| Duty | Why a bank cares |
| --- | --- |
| Routing | `/payments` goes to the payments service. `/profile` goes to the profile service. Callers do not track internal addresses |
| Authentication | Check the token, certificate, or API key at the door |
| Authorisation help | Reject a caller that is not subscribed to this API. Fine-grained "does this customer own this account?" still belongs in the service |
| TLS termination | Decrypt HTTPS at the door, or re-encrypt inward, depending on the design |
| Rate limiting and quotas | A partner gets an agreed number of calls. A burst cannot drown the core |
| Throttling | Slow a caller down rather than only cutting them off |
| Request size limits | Reject huge payloads that would waste the service |
| Logging and analytics | Who called which API, how often, and how long it took |
| Correlation id | Stamp a trace id so operations can follow the call |
| Transformation | Turn an old SOAP service into a JSON reply the mobile app understands. This is an **adapter** or **facade** role |
| Version routing | Send `/v1` to the old service and `/v2` to the new one during a migration |
| IP allow lists | For partner APIs, accept calls only from agreed addresses |
| Response caching | For safe, rarely changing reads |
| Composition, sometimes | Call two services and combine the answer. Useful, and dangerous if the gateway becomes a second application full of business rules |

### What the gateway should not become

Business rules of the ledger, such as "is there sufficient balance?", belong in the service that owns them. If every rule creeps into the gateway, the gateway becomes a bottleneck and a risk. A good gateway is a firm receptionist. It is not every officer in the branch.

### Names you might see

Kong, Apigee, Amazon API Gateway, Azure API Management, NGINX, and IBM API Connect are products in this category. The exam cares about the role, not a brand.

### Gateway, ESB, and load balancer

| Piece | Main job |
| --- | --- |
| Load balancer | Spread traffic across copies of the same service and skip dead copies |
| API gateway | One controlled entry for many APIs: identity, limits, routing, logs |
| ESB | Older enterprise bus that routes and transforms messages among internal systems |

One product can overlap these roles. Explain them by duty and you will answer clearly.

**Say it in one line:** The API gateway is the single, policed entrance to the bank's services. It authenticates, routes, limits, and logs. The services still own the business decisions.

---

## How callers prove themselves

| Method | Simple picture | Typical use | Care |
| --- | --- | --- | --- |
| API key | A long secret password for an application | A server-side partner integration | If it leaks, anyone can call as that partner. Do not embed it in a mobile app |
| Client certificate | A certificate proving the calling machine | Bank-to-bank or bank-to-government links | Protect the private key. Rotate certificates before expiry |
| OAuth 2.0 | A customer grants an app limited access without giving the app their banking password | "Allow this account-aggregator or fintech to read my statement" | The app receives a token with a scope and an expiry, not the customer's password |
| OpenID Connect | A layer on OAuth for login identity | "Log in with a central identity provider" | It answers who the person is |
| JWT | A signed token presented on each call | Mobile API calls after login | Short life, signature check, no secrets inside |
| Session cookie | The browser session described in the security chapter | Traditional web channel | Needs CSRF protection |
| HMAC signature | The caller signs the request body with a shared secret | Some payment callbacks | The receiver recomputes and compares |
| mTLS | Mutual TLS. Both the caller and the server present certificates | Sensitive system-to-system calls | Stronger than a password on the wire |

### OAuth words, slowly

OAuth confuses honest people. Keep four roles:

| Role | Bank story |
| --- | --- |
| Resource owner | The customer |
| Client | The outside app that wants access |
| Authorisation server | The bank component that lets the customer approve and then issues tokens |
| Resource server | The API that holds the statement or the balance |

The customer approves a **scope**, such as "read accounts" and not "transfer money". The app gets an **access token** that expires. A **refresh token** can ask for a new access token without bothering the customer again, until the grant is revoked.

The customer's password is typed into the bank's own page, not into the third-party app. That separation is the point of the protocol.

**Account Aggregator**, in the Indian financial system, is a consent-based way to share financial data among regulated entities. You do not need its full architecture here. The exam-relevant link is: APIs plus customer consent plus limited tokens, rather than sharing internet-banking passwords.

---

## Design habits that keep APIs safe and usable

- **Least data.** Return the fields the caller needs. An internal customer-master dump is not a balance API.
- **Object-level checks.** Every request that names an account must check that this caller may touch this account. This is the API form of broken access control. OWASP calls it BOLA, broken object-level authorisation.
- **Validation.** Amount, currency, date, and account format are checked on the server.
- **Pagination.** A statement API returns a page of rows, not ten years in one response.
- **Versioning.** Add `/v2` or another clear strategy when a change would break old callers.
- **Idempotency keys** for payments.
- **Meaningful errors** that help a genuine caller and do not teach an attacker how the core is built.
- **Timeouts.** Do not wait forever for a slow downstream system.
- **Documentation.** A contract the partner can read.
- **Backward compatibility.** Adding an optional field is usually safe. Removing or renaming a field breaks callers.
- **Deprecation.** Announce the retirement of an old version and give callers time.

### Webhook

A normal API call is the client asking the server. A **webhook** is the server calling the client later, when something happens. Example: the payment system notifies the merchant "this UPI collection has succeeded". The merchant exposes a URL. The bank must sign that notification so the merchant can verify it is really the bank.

---

## A full picture

```
Mobile app
    |
    | HTTPS, with a customer token
    v
API gateway
    |  checks token, limit, route, trace id
    v
Payments API  (REST, JSON)
    |
    | may call inward
    v
Core banking interface  (might still be SOAP or a host message)
    |
    v
Ledger
```

The customer sees one app. The app sees one gateway. The gateway sees services. A service may still speak an older dialect to the core. That is normal in a bank and is a sign of controlled evolution.

---

## API mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "REST is a protocol like SOAP" | REST is a style. SOAP is a protocol with an XML envelope |
| "GET can be used to transfer funds if the URL is secret" | GET is for safe reads. Fund transfer is a POST or another non-safe operation, with authentication |
| "401 and 403 are the same" | 401 is an identity failure. 403 is a permission failure |
| "The API gateway replaces authorisation inside the service" | The gateway can reject unknown callers. The service must still check ownership of the account |
| "A WSDL describes a REST API" | WSDL describes SOAP. REST is commonly described with OpenAPI |
| "Stateless means the bank forgets the customer" | It means the request carries its credential, and any server copy can process it |
| "OAuth means giving the partner your banking password" | OAuth exists so that you do not do that. The partner gets a limited token |

## Remember these lines

- API = a programming interface. Web service = that idea offered over the network with web standards.
- REST = resources, HTTP methods, usually JSON, stateless calls.
- SOAP = XML envelope, WSDL contract, formal enterprise style.
- GET reads. POST acts. Repeating a payment must not double-debit. Use an idempotency key.
- 401 means not authenticated. 403 means not allowed.
- The API gateway routes, authenticates, rate-limits, and logs at the front door.
- OAuth lets a customer grant limited access without sharing the banking password.

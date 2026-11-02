# 2. Back-end Technologies and Server-side Processing

## The one story

The customer taps "Show balance" on the phone. The phone only knows the account number that was typed. It does not hold the bank's ledger.

The request travels to a computer in the bank's data centre. That computer:

1. Checks that the request is genuine.
2. Confirms that this customer may see this account.
3. Asks the core banking database for the balance.
4. Applies business rules, such as hiding internal codes and formatting the amount.
5. Sends a small, safe answer back to the phone.

All of that work happens on the **server**. It is **server-side processing**. The programs that do it are the **back end**.

**Say it in one line:** The back end is the protected office behind the screen. It holds the rules, the secrets, and the connection to the bank's data.

---

## Words that build the picture

| Word | Simple meaning |
| --- | --- |
| Client | The customer's device and the front end running on it |
| Server | A computer that waits for requests and sends responses. Also the program doing that job |
| Request | The question sent by the client. Example: "What is the balance of account 123?" |
| Response | The answer sent by the server. Example: "The balance is ₹ 25,000" |
| Back end | The server-side part of the application: programs, rules, and data access |
| Server-side processing | Work done on the server after a request arrives and before the response leaves |
| Front end | The part on the client. Covered in the previous chapter |

A useful memory: the client proposes, the server decides.

---

## What the server actually does with one request

Follow a fund-transfer request from start to finish.

1. **Receive.** A web server accepts the connection.
2. **Read.** It reads the method, the address, the headers, and the body. The body may contain from-account, to-account, and amount.
3. **Identify.** It checks who is calling. This may be a session, a token, or a certificate.
4. **Authorise.** Being logged in is not enough. The program checks that this user may debit this account and that the amount is within limit.
5. **Validate.** It checks types, ranges, and formats again, even if the browser already checked them.
6. **Apply business rules.** It may check the cutoff time, the working day, the beneficiary cooling period, and sufficient balance.
7. **Talk to other systems.** It may call core banking, an antifraud system, and an SMS gateway.
8. **Persist.** It stores the transaction in a database so the record survives a restart.
9. **Respond.** It returns a success receipt or a clear failure. It also writes a log for audit.
10. **Clean up.** It closes or returns the database connection to a pool, and it does not leave half-finished work. If a step fails, it rolls back.

That list is server-side processing in a bank. The screen only displayed the outcome.

---

## Web server and application server

People mix these two. Keep them apart.

### Web server

A **web server** accepts HTTP connections and serves content.

It is good at:

- Serving files such as HTML, CSS, JavaScript, and images.
- Receiving a request and forwarding it to the application.
- Handling many connections, encryption, and basic routing.

Names you should recognise:

| Name | Note |
| --- | --- |
| Nginx | Pronounced "engine-x". Very common as a fast web server and reverse proxy |
| Apache HTTP Server | A long-standing, widely used web server. Often just called Apache |
| IIS | Internet Information Services. Microsoft's web server on Windows |

### Application server

An **application server** runs the bank's own program: the balance logic, the transfer logic, the connection to databases and queues.

Names you should recognise:

| Name | Note |
| --- | --- |
| Apache Tomcat | Runs Java web applications. Very common |
| JBoss / WildFly | A Java application server family |
| WebLogic | Oracle's Java application server, seen in large enterprises |
| WebSphere | IBM's application server, also seen in large banks |
| .NET runtime on IIS | A common Microsoft path |

### How they sit together

A typical path:

`Customer → Nginx (web server / reverse proxy) → Tomcat (application) → Database`

The web server is the receptionist. The application server is the officer who does the actual work. In smaller setups one product does both jobs. In a bank they are often separate so each can be scaled and secured on its own.

**Reverse proxy** means a server that sits in front of the application and forwards client requests inward. The client talks to the proxy. The proxy talks to the application. This hides internal machines and can add encryption and load sharing.

---

## Languages and runtimes used on the server

The syllabus says "back-end technologies", so recognise the family, not one brand only.

| Technology | Plain picture |
| --- | --- |
| Java | The most common language in large Indian bank systems. Runs on the JVM, the Java Virtual Machine. Servlets and Spring are part of this world |
| Spring and Spring Boot | A Java framework family that helps build web services, security, and database access with less repetitive code |
| Jakarta EE / Java EE | The enterprise Java platform. Servlets, JSP, and older bank applications live here |
| Servlet | A Java class that receives an HTTP request and produces a response. It is a basic building block |
| JSP | JavaServer Pages. HTML with Java embedded, used to build pages on the server. Older style. New work prefers APIs plus a separate front end |
| Node.js | JavaScript on the server. One language on both sides. Good for many small simultaneous requests |
| Python | Used with frameworks such as Django or Flask. Strong in data and automation, also used for web APIs |
| PHP | Powers many traditional websites. Less dominant in core banking, still seen in web portals |
| C# and ASP.NET | Microsoft's back-end path |
| SQL | The language used to ask questions of a relational database. The back end uses it, or an ORM uses it for the back end |

**ORM** means Object-Relational Mapping. It lets Java or Python objects be saved as database rows without the programmer hand-writing every SQL sentence. Hibernate is a famous Java ORM.

You are not expected to code these. You are expected to know that the business rules run in such a program, on a server, close to the data.

---

## Dynamic pages and the two ways to build a screen

**Static content** is the same for every customer. A logo file is static.

**Dynamic content** changes with the user and the moment. Your balance is dynamic. The server, or the front end after asking the server, builds it.

Two styles:

| Style | What happens | Where you meet it |
| --- | --- | --- |
| Server-side rendering | The server builds the finished HTML and sends it. The browser mostly displays it | Older portals, some secure pages, search-friendly public pages |
| Client-side rendering | The server sends data, often JSON. JavaScript in the browser builds the screen | Modern SPAs and mobile-like web apps |
| A mix | The server sends the first HTML quickly, then JavaScript takes over | Many current sites |

**Server-side processing** is required in both styles whenever money, identity, or private data is involved. Even if the browser draws the screen, the decision is made on the server.

**Template** means a page shape with blanks. The server fills the blanks with this customer's name and balance. JSP, Thymeleaf, and similar tools are template engines.

---

## State: how the server remembers a customer

HTTP, the web's basic conversation, is **stateless**. Each request is a fresh letter. The server does not automatically remember the previous letter.

A balance enquiry followed by a transfer needs memory: "this is the same logged-in customer". That memory is **state**, and the usual tool is a **session**.

### Session

1. The customer logs in successfully.
2. The server creates a session: a private record with a random session id, the user id, the login time, and maybe the list of allowed accounts.
3. The server sends the session id to the browser, usually inside a cookie.
4. On the next request the browser sends that cookie back.
5. The server looks up the session and knows who is calling.

The session data itself should stay on the server or in a protected server-side store. The browser should hold only the random id, not the balance, the role, and the account list in a form the user can edit.

### Cookie

A **cookie** is a small name-value pair stored by the browser and sent to the matching site.

For sessions, the cookie is a claim ticket. The coat, which is the real session, stays in the cloakroom, which is the server.

Important cookie flags are explained in the security chapter: `HttpOnly`, `Secure`, and `SameSite`.

### Other ways to carry identity

- A **token**, often a JWT, travels with each request. The server checks the token's signature. This suits APIs and mobile apps.
- **Basic authentication** sends a user id and password with requests. It is simple and rarely the right choice for a customer-facing bank app.
- **Certificates** identify a machine or a partner system. They are common between banks and between a branch system and a central system.

**Say it in one line:** HTTP forgets. A session or a token is how we carefully remember who the caller is.

---

## Middleware

**Middleware** is software that sits in the middle and does a repeated job so every business program does not reimplement it.

Examples:

- A security layer that checks the token before the transfer code runs.
- A logging layer that records every request.
- A message queue that carries work between systems.
- An integration layer that speaks to core banking, credit bureaus, and government interfaces.

In Java people also say "middleware" for the application-server layer itself. In modern architecture the word often means the helpers between the screen and the systems of record.

A **message queue** stores a message until another program is ready to process it. If the SMS gateway is briefly down, the "send OTP" message can wait in the queue instead of being lost. Kafka and RabbitMQ are well-known queue and streaming products. IBM MQ is familiar in older bank estates.

---

## MVC — a way of organising back-end code

**MVC** means Model, View, Controller.

| Part | Job | Bank example |
| --- | --- | --- |
| Model | The data and the business rules | Account, amount, "balance must be sufficient" |
| View | What is shown to the user | The receipt page or the JSON that the screen will draw |
| Controller | The traffic manager. It receives the request, calls the model, and chooses the view | The code that handles `POST /transfer` |

Why it exists: if rules, screens, and request handling are mixed in one file, a small change becomes dangerous. MVC separates them.

In an API-only back end, the "view" is often a JSON response rather than an HTML page. People then talk about **service layer** and **repository layer**:

- Controller receives HTTP.
- Service applies business rules.
- Repository reads and writes the database.

The names change. The idea stays: keep entry, rules, and storage apart.

---

## Databases, as the back end sees them

The back end is the door to the data. The front end should not open the database directly.

| Word | Simple meaning |
| --- | --- |
| Database | An organised store of data |
| Relational database | Data in tables with rows and columns, linked by keys. Oracle, PostgreSQL, MySQL, SQL Server, and DB2 are names you will meet. Core banking often uses a major relational database |
| Table | One subject, such as customers or transactions |
| Row | One record |
| Column | One field, such as amount |
| Primary key | The column, or set of columns, that uniquely identifies a row. An account number can be one |
| Foreign key | A value that points to a primary key in another table. A transaction row stores the account number of the account it belongs to |
| SQL | Structured Query Language. `SELECT` reads, `INSERT` adds, `UPDATE` changes, `DELETE` removes |
| Transaction (database) | A group of steps that must all succeed or all be undone. Debit one account and credit another is a classic pair |
| Commit | Make the transaction permanent |
| Rollback | Undo the transaction because a step failed |
| ACID | The four promises of a reliable database transaction, explained below |
| Index | An extra lookup structure, like a book index, so the database can find rows faster |
| NoSQL | Databases that are not classic tables. Useful for some large or flexible data. The system of record for balances is still usually relational |

### ACID, slowly

- **Atomicity** — all or nothing. You never debit without the matching credit.
- **Consistency** — the database moves from one valid state to another. Rules such as "balance cannot go negative" still hold.
- **Isolation** — two transfers running at the same moment do not see each other's half-finished work.
- **Durability** — after commit, the result survives a power failure.

A senior manager's version: ACID is why the ledger can be trusted when many customers act at once.

### Connection pool

Opening a database connection is expensive. A **connection pool** keeps a set of open connections ready. The application borrows one, does the work, and returns it. This is both a back-end technique and a performance technique.

---

## Authentication and authorisation

These two words are a favourite exam pair.

| Word | Question it answers | Example |
| --- | --- | --- |
| Authentication | Who are you? | Customer enters credentials and an OTP, and the server believes the identity |
| Authorisation | What are you allowed to do? | This customer may view the account but may not add a new beneficiary today |

A teller and a customer can both be authenticated. Their authorisation is different. A teller may post a cash deposit. A customer may not open the teller's screen.

**SSO**, single sign-on, means one login grants entry to several applications the person is allowed to use. Staff portals often need this so an officer does not keep ten passwords.

**MFA** or **2FA** means multi-factor or two-factor authentication. Something you know (password), something you have (phone or token), or something you are (fingerprint). Internet banking that asks for a password and an OTP is using two factors.

OTP means one-time password. It is valid for a short time and for one use.

---

## Business rules live on the server

Examples of rules that must be server-side:

- This user may debit only accounts where they are an operating authority.
- A newly added beneficiary cannot receive an immediate high-value transfer.
- The amount cannot exceed the channel limit for net banking.
- The transaction must be inside the cutoff for that payment system.
- A staff user cannot approve their own transaction.
- Personal data in the response must match what this role is allowed to see.

If a rule exists only in the front end, it is a hint, not a control.

---

## Logs, errors, and audit

Server-side processing includes evidence.

- An **application log** helps engineers see failures. It must not contain full passwords, OTPs, card numbers, or unnecessary customer data.
- An **audit log** records who did what, to which account, when, and from where. For a bank this is part of control, not a nice extra.
- An **error to the customer** should be clear and calm: "We could not complete the transfer. Your account was not debited. Reference 1842."
- An **error to the attacker** should not be a map of the system. "SQL exception at line 88 in AccountDao" is too much detail for a public response.

**Idempotency** is a grown-up word worth knowing. An idempotent operation can be repeated safely and still have one result. If the customer's phone retries a transfer because the network blinked, the server should recognise the same request id and not debit twice. Payment systems care deeply about this.

---

## A staff example and a customer example

**Customer example.** Yono-style balance enquiry: the app sends the request, the API layer authenticates the session, the service checks account entitlement, the core system returns the balance, and the API returns a small JSON. The app never receives the entire customer master table.

**Staff example.** A branch officer searches a customer. The request carries the officer's identity. The server checks the officer's role and branch powers, writes an audit record of the search, and returns only the fields that role may see. The search screen is front end. The power to see the data is back end.

---

## Back-end mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "The web server and the database are the same thing" | The web server handles HTTP. The application applies rules. The database stores data |
| "Stateless HTTP cannot support login" | HTTP is stateless. Sessions and tokens add memory in a controlled way |
| "Authentication and authorisation are the same" | Authentication identifies. Authorisation permits |
| "Server-side rendering means there is no front end" | The browser still displays the page. The HTML was built on the server |
| "A queue loses the message if the receiver is busy" | A queue holds the message until the receiver can take it |
| "Rollback deletes yesterday's committed work" | Rollback undoes the current uncommitted transaction |

## Remember these lines

- Back end = server-side programs, rules, integrations, and data access.
- Web server receives HTTP. Application server runs the bank's logic. Database stores the ledger.
- Server-side processing = validate, authenticate, authorise, apply rules, update data, respond, and log.
- HTTP is stateless. Sessions and tokens supply continuity.
- MVC separates request handling, business rules, and presentation.
- ACID is the database promise that paired debits and credits stay trustworthy.
- Client checks are helpful. Server checks are decisive.

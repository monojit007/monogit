# 6. Performance and Scalability Techniques

## The one story

On a quiet Tuesday, the balance page opens in one second. On salary evening, ten lakh customers open it together. If the bank only planned for Tuesday, salary evening becomes a failure.

Two related words:

- **Performance** means how quickly and smoothly the system serves the customers it has now. Response time, errors, and the load on machines are the signs.
- **Scalability** means the system's ability to take more load by adding resources, without a redesign and without collapsing.

A fast system for ten users is performance. A system that stays fast for ten lakh users is scalability.

**Say it in one line:** Performance is speed and smoothness today. Scalability is the ability to keep that smoothness as demand grows.

---

## The measurements, because "it feels slow" is not a control

| Measure | What it counts | Plain example |
| --- | --- | --- |
| Response time or latency | How long one request takes from send to complete answer | The balance appeared in 800 milliseconds |
| Throughput | How much work is completed in a period | 2,000 balance enquiries answered per second |
| Concurrent users | How many are in the middle of work at the same moment | 50,000 sessions active now |
| Error rate | What share of requests fail | 0.2 percent of transfers returned an error |
| Availability | The share of time the service is usable | 99.9 percent in a quarter |
| Apdex or a similar score | How many users found it fast enough versus frustrating | A management view of the same latency data |
| CPU, memory, disk, network | Whether the machine itself is saturated | The database CPU stays near 100 percent |

**Percentile** matters more than average. If the average is 1 second and the slowest 5 percent take 20 seconds, those customers experience a broken app. People say **p95** or **p99**: 95 or 99 percent of requests finished within this time.

**SLA** is a service level agreement, a promised target. **SLO** is the objective you aim at internally. **SLI** is the indicator you measure. You can remember them as promise, target, and measurement.

### The numbers people quote for availability

| Target | Rough downtime in a year |
| --- | --- |
| 99 percent | About 3.6 days |
| 99.9 percent, "three nines" | About 8.7 hours |
| 99.99 percent, "four nines" | About 52 minutes |

These are planning figures, not a claim about any one bank. The point is that each extra nine is much harder.

---

## First find the bottleneck

A **bottleneck** is the narrowest part. Adding application servers does nothing if the database is already exhausted. Adding memory does nothing if the page is slow because it downloads a 5 MB image.

A disciplined order:

1. Measure, do not guess.
2. Find the slowest necessary step.
3. Improve that step.
4. Measure again.

**Profiling** means inspecting where time and resources go. **Load testing** means simulating many users before the real salary day. **Stress testing** pushes beyond expected peak to see how the system fails. **Soak testing** runs a realistic load for a long time to catch slow leaks, such as memory that is never released.

---

## Technique 1 — Make the page itself lighter

These help the front end. The customer feels them immediately.

| Technique | What you do | Why it helps |
| --- | --- | --- |
| Minification | Remove unnecessary spaces and long names from CSS and JavaScript files that are sent to the browser | Smaller download |
| Compression | Gzip or Brotli squeezes the file on the wire. The browser unpacks it | Smaller download. Different from minification. You can do both |
| Image optimisation | Right size, modern formats, no giant photo where a small logo is needed | Images are often the heaviest part of a page |
| Lazy loading | Load a picture or a section when the customer is about to see it, not all at the first instant | The first screen appears sooner |
| Fewer requests | Combine where it helps, and avoid a page that calls fifty small files one by one | Each request has an overhead |
| Cache static files | Tell the browser it may reuse the logo and the CSS for a period | The next page does not download them again |
| Critical content first | Show the heading and the balance area before secondary widgets | The page feels ready |
| CDN | Described below | The file comes from a nearby copy |
| Avoid heavy work on the main thread | A script that blocks the browser makes taps feel dead | The screen stays responsive |

**Minify** and **compress** are a common mix-up. Minify edits the code to be shorter. Compress packs the bytes for travel. The browser needs the original meaning either way, and both techniques preserve meaning.

---

## Technique 2 — Caching

A **cache** is a nearby copy of something expensive to fetch or compute.

Levels:

| Cache | Where | Good for | Dangerous for |
| --- | --- | --- | --- |
| Browser cache | The customer's device | Logos, CSS, scripts | Personal balances |
| CDN cache | Edge locations near customers | Public, shared content | Anything private to one customer |
| Application cache | In the service's memory or a cache server such as Redis | Branch lists, product parameters, a very short-lived repeated read | A debit decision based on a stale balance |
| Database cache | Inside the database engine | Recently used data pages | You do not manage this the way you manage Redis, but you should know it exists |

**Redis** and **Memcached** are well-known in-memory cache products. In-memory means the data sits in RAM, which is fast.

Cache rules a manager should insist on:

- Every cached item needs a **TTL**, a time to live. After that, it is fetched again.
- There must be a way to **invalidate** an item when the source changes. If the branch timing changes, the old timing must not linger all day.
- Private data must be cached only for that user and must be protected like the original.
- The first request after expiry is a **cache miss**. A miss goes to the source. A **cache hit** is served from the copy.

**Cache stampede**: many requests notice the miss at once and all rush to the database. Protection is to let one request refresh the value while others wait or receive the old value briefly.

---

## Technique 3 — CDN

**CDN** means Content Delivery Network. Copies of static content are placed in many locations. A customer in one city gets the logo from a nearby node instead of from the bank's central data centre.

It reduces distance, which reduces latency, and it takes load off the origin servers.

A CDN is excellent for images, app bundles, and public pages. The personal API call that posts a transfer still goes to the bank's protected systems. Do not "put the ledger on a CDN".

---

## Technique 4 — Database performance

Many banking slowdowns end in the database.

| Technique | Plain meaning |
| --- | --- |
| Index | A lookup aid. Finding an account by account number becomes a direct lookup instead of reading every row. Too many indexes slow down writes, so index what you search |
| Query design | Ask only for the columns and rows you need. A statement for one month should not scan twenty years |
| Pagination | Return 20 transactions, then the next 20 |
| Avoid N+1 | Do not ask the database once for a list and then once per row again. Fetch the needed data in a planned way |
| Connection pool | Reuse open connections instead of creating one per click |
| Read replica | A copy that serves reads, while the primary accepts writes. The copy may lag by a short time. Do not read a balance from a lagging copy if you are about to debit |
| Partition or shard | Split a huge table by a key, such as date or customer range, so one query touches less data |
| Archival | Move old transactions that are rarely read to an archive store, under the bank's retention rules |
| Explain plan | The database's own explanation of how it will run a query. Engineers use it to see a missing index |

**Locking** keeps two debits from using the same rupee. Necessary locks protect integrity. Locks held too long make everyone queue. Performance work must not remove a lock that protects the ledger. This is a business risk, not only an engineering detail.

---

## Technique 5 — Load balancing

A **load balancer** distributes requests across several healthy servers.

Ways of choosing a server:

| Method | Idea |
| --- | --- |
| Round robin | Give one request to each server in turn |
| Least connections | Prefer the server currently handling fewer requests |
| Least response time | Prefer the server answering fastest |
| Hash or sticky session | Send the same customer to the same server. Useful only when that server holds their session in memory |

**Sticky sessions** are a comfort and a limit. If that one server dies, the session dies. A shared session store, or stateless tokens, lets any server take the next request. That is better scaling.

**Health check**: the balancer pings each server. A server that fails the check is taken out until it recovers.

Load balancers also terminate TLS, sit in a pair so the balancer itself is not a single point of failure, and live at more than one layer. You can balance web servers, and those servers can balance calls to downstream services.

---

## Technique 6 — Scaling up and scaling out

| | Vertical scaling, scale up | Horizontal scaling, scale out |
| --- | --- | --- |
| What you add | A bigger machine: more CPU, more RAM, faster disk | More machines beside the existing ones |
| Simple picture | Replace the counter clerk with a faster clerk | Open more counters |
| Limit | There is a biggest machine you can buy, and one machine is still one failure | You must design so machines can share work. A load balancer and stateless services help |
| Bank habit | Often used for a primary database, within a limit | The usual path for web and application servers |

**Auto-scaling** means the platform adds or removes copies based on load, within limits you set. The limits matter. Unlimited growth can multiply a runaway bug and a cloud bill.

**Capacity planning** is the management act: know the peak, test above it, and have a headroom policy. Festival days, salary days, and product launches are planned events, not surprises.

---

## Technique 7 — Asynchronous work and queues

Not every step needs to finish before the customer sees "accepted".

A transfer may:

1. Check the essential rules.
2. Post, or reserve, the money.
3. Reply to the customer with a reference.
4. Send SMS, update analytics, and notify a downstream system through a queue.

The SMS can wait a few seconds in a **queue**. The customer should not stare at a spinner because an SMS gateway is slow.

Use asynchronous work for side tasks. Do not use it to hide an unfinished debit and then tell the customer success. The status shown must be honest: accepted, pending, successful, or failed.

**Back pressure** means when the consumer is overloaded, the producer slows down, instead of piling up infinite work until something breaks.

---

## Technique 8 — Connection and thread care

Each request needs a worker and often a database connection. If you allow infinite concurrent work, the server spends all its time switching and waiting.

- A **pool** limits and reuses connections.
- **Timeouts** free workers stuck on a dead downstream system.
- **Circuit breaker**: if the fraud system is failing every call, stop calling it for a short period and fail fast or use a defined fallback. Then try again carefully. This prevents a sick dependency from exhausting the caller.
- **Bulkhead**: separate pools for separate journeys, so a flood of statement downloads cannot consume every worker the payment path needs. The name comes from ship compartments that keep one leak from sinking the ship.
- **Retry with care**: retry a read. Retry a payment only with an idempotency key. Retrying instantly and forever creates a storm.

These four words — timeout, retry, circuit breaker, bulkhead — are the practical heart of a resilient service.

---

## Technique 9 — Code and design habits

- Do less per request. A balance call should not rebuild the customer's entire profile.
- Avoid repeated calls. Fetch a reference data table once and cache it.
- Paginate.
- Push only the fields the screen needs.
- Use asynchronous calls where waiting adds no value.
- Keep session data small.
- Test with production-like data volumes. A query that is fine for 100 rows can be fatal for 100 million.

**Performance budget**: agree a limit, such as "the account home calls at most five APIs and finishes within two seconds on a normal phone network". New features must fit the budget or the budget is consciously changed.

---

## Technique 10 — Network and protocol choices

- **HTTP/2** and **HTTP/3** carry many requests more efficiently than old HTTP/1.1. The exam point is that newer HTTP versions reduce overhead. You do not need packet detail.
- Keep important systems close in network terms. An application that chats to its database across a slow long link will feel slow no matter how elegant the code is.
- **DNS latency** and TLS handshake cost are real. Session resumption and connection reuse reduce repeat costs.
- Compression helps text. It does little for already-compressed images, and it costs CPU. Measure.

---

## Scalability shapes

| Shape | Meaning |
| --- | --- |
| Scale up | Bigger box |
| Scale out | More boxes |
| Scale down or in | Remove capacity when the peak has passed, especially in cloud, so you do not pay forever for salary-day size |
| Elasticity | The ability to grow and shrink with demand |
| Linear scalability | Double the servers, roughly double the throughput. Real systems fall short of perfect linear scaling because of shared databases and coordination |

**Amdahl's idea, in one sentence:** the part you cannot parallelise limits the benefit of adding machines. If every request must take a lock on one row in one database, more web servers eventually just wait in a longer queue.

---

## A salary-day plan, using the techniques together

1. Capacity numbers are agreed weeks earlier. Load tests run at 1.5 or 2 times expected peak.
2. Static files sit on a CDN and are cached by browsers.
3. A load balancer spreads traffic across many stateless application servers. Auto-scaling has a ceiling.
4. Reference data is cached in Redis with a short TTL.
5. Balance reads go to a path designed for read volume. Debits go to the system of record with proper locking.
6. Statements are paginated.
7. SMS and analytics go to queues.
8. The fraud service has a timeout and a circuit breaker so a fraud-system outage has a planned response, not an accidental outage of every channel.
9. A dashboard watches p95 latency, error rate, CPU, and queue depth.
10. A runbook says who decides to shed load, pause a campaign, or fail over.

**Load shedding** means deliberately refusing or simplifying less important work to protect the core journey. Showing a "please try the statement after some time" message can be wiser than letting the payment path die with it.

---

## Performance mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "Scalability and performance are the same word" | Performance is current speed. Scalability is behaviour as load grows |
| "A cache should hold everything, including live balances used for debit" | Cache what is shared and tolerant of a little age. The ledger decision needs current data |
| "Sticky sessions are required for all web apps" | They are one option. Shared sessions or tokens scale more cleanly |
| "Adding servers always fixes slowness" | Only if the added tier is the bottleneck |
| "Retries make a system reliable" | Blind retries can duplicate payments and amplify failures |
| "Minification encrypts the JavaScript" | It only shortens it. The customer can still read the logic |
| "A CDN hosts the core database" | A CDN distributes content close to users. The ledger stays in the bank's controlled data tier |

## Remember these lines

- Measure latency percentiles, throughput, errors, and saturation. Then fix the bottleneck.
- Light pages, compression, minification, lazy loading, and browser caching help the first impression.
- Cache and CDN serve safe repeated content from nearby.
- Indexes, pagination, pools, and careful reads keep the database upright.
- Load balancers plus horizontal scaling multiply application capacity.
- Queues move slow side work off the customer's waiting time.
- Timeouts, circuit breakers, bulkheads, and idempotent retries keep one failure from becoming a flood.

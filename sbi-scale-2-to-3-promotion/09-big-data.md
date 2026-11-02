# 9. Big Data

The syllabus line is: core 5Vs, architecture and ecosystems, distributed storage and processing, analytics, data governance.

## The one story

A branch register can be read by one person. A bank's world is larger than a register.

In one day the bank may see:

- crores of transaction rows from core banking, cards, UPI, and ATMs,
- mobile-app clicks,
- call-centre notes,
- SMS delivery logs,
- fraud alerts,
- and files arriving from other institutions.

Some of this fits a traditional database and should stay there. The ledger is a traditional database problem, because every rupee must be exact.

Some of it is so large, so fast, or so mixed in shape that a single ordinary server and a single table design struggle. That is the neighbourhood of **big data**.

**Say it in one line:** Big data is data so large, fast, or varied that we use a distributed ecosystem to store it, process it, analyse it, and govern it. The bank's ledger still needs exact transactions. Big data helps the bank understand the wider picture.

---

## The core 5Vs

These five words are the definition examiners expect. Each V is a pressure. Big data is what we do when those pressures show up together.

### 1. Volume — how much

Volume means quantity.

One account statement is small. Ten years of transactions for tens of crores of accounts, plus logs, is large. The data may be terabytes or petabytes.

Simple scale, so the words are not empty:

| Unit | Rough size |
| --- | --- |
| Megabyte | A long document or a photo |
| Gigabyte | A film, or a small database |
| Terabyte | A thousand gigabytes. A large operational store |
| Petabyte | A thousand terabytes. A serious analytics estate |

You do not need to memorise the bank's actual size. You need to know volume means "too much to treat as one small file on one small machine".

Volume pushes us toward many machines and toward storage that can grow by adding machines.

### 2. Velocity — how fast

Velocity means the speed at which data arrives and the speed at which we may need to use it.

Examples:

- Card authorisations and UPI payments arrive continuously.
- A fraud decision may have to happen while the payment is still in flight.
- An end-of-day file arrives in one burst. That is also velocity, just in a wave.

Two styles follow from velocity:

| Style | When the result is ready | Example |
| --- | --- | --- |
| Batch | After a collection window. Minutes, hours, or overnight | A morning book of yesterday's channel failures |
| Real time or near real time | During the event or within seconds | Decline a suspicious card swipe before approval |
| Streaming | A continuous flow that is processed as it flows | A live dashboard of UPI success rates |

Near real time means "fast enough for this decision", not "the speed of light". Be honest about the business need. A monthly MIS report does not need millisecond streaming.

### 3. Variety — how many shapes

Variety means data does not all come as neat columns.

| Kind | Shape | Bank example |
| --- | --- | --- |
| Structured | Tables with defined columns | A transaction row: date, account, amount, status |
| Semi-structured | Has labels, and the shape can flex. JSON and XML | An API log, a mobile event |
| Unstructured | No fixed columns | A complaint email, a scanned KYC document, a call recording |

A relational database loves structured data. Big-data tools exist largely so the other shapes are not thrown away.

### 4. Veracity — how much we can trust it

Veracity means trustworthiness.

Data can be wrong, duplicated, late, missing, or contradictory. A mobile number updated in one system and stale in another is a veracity problem. A sensor or a log that drops events is a veracity problem. A customer name spelt three ways is a veracity problem.

If volume is high and veracity is low, you have a large pile of misleading evidence. Analytics on dirty data produces confident nonsense.

Veracity is why data quality work is part of big data, not a clerical side issue.

### 5. Value — why we keep it

Value means the data should help a decision or a service.

Storing everything forever has a cost: money, risk, and clutter. Value asks:

- Does this feed fraud prevention, service quality, capital planning, or a better customer journey?
- Who will use it?
- Is the value worth the privacy risk of holding it?

Data kept "because it might be useful someday", with no owner and no control, usually fails the value test and fails governance too.

### The five in one sentence each

| V | Sentence |
| --- | --- |
| Volume | There is a great quantity of data |
| Velocity | It arrives quickly or must be used quickly |
| Variety | It comes in tables, files, logs, text, and more |
| Veracity | We must be able to trust it, or know that we cannot |
| Value | It must be worth collecting and must serve a purpose |

Some writers add more Vs: variability, visualisation, or venue. The syllabus says the core five. If a question says 5Vs, give these five. If it says 3Vs, the original three were volume, velocity, and variety. Veracity and value were added as the idea matured.

**Say it in one line:** Volume is size, velocity is speed, variety is shape, veracity is trust, and value is usefulness.

---

## Architecture: the path data walks

Architecture here means the stages data passes through. Remember one pipeline.

```
Sources
  → Ingestion
    → Storage
      → Processing
        → Analytics and models
          → Serving
            → Users and channels
```

Governance and security wrap every stage. They are not a stage at the end.

### 1. Sources

Where data is born.

- Core banking, loans, deposits, cards, payments.
- Channels: branch, ATM, internet, mobile, contact centre.
- External: credit bureaus, payment systems, market rates, government interfaces where permitted.
- Machine data: application logs, security logs.

A **system of record** is the official source for a fact. The core ledger is the system of record for a balance. An analytics copy is not. If they disagree, the system of record wins for financial truth, and the disagreement is a defect to fix.

### 2. Ingestion

**Ingestion** means bringing data in, in a controlled way.

| Mode | How | Tool family you can name |
| --- | --- | --- |
| Batch ingestion | A file or a scheduled extract | Sqoop for database-to-Hadoop extracts, ordinary file transfer, ETL jobs |
| Stream ingestion | Events as they happen | Kafka, Flume for log streams |
| CDC, change data capture | Capture inserts and updates from a database as they occur | Database log readers feeding a stream |

**ETL** means Extract, Transform, Load. Pull data out, clean and reshape it, then load it into the destination.

**ELT** means Extract, Load, Transform. Load the raw data first, then transform inside the destination. This is common in a data lake, because you keep the raw form and shape later copies for each use.

A **schema** is the expected shape. **Schema on write** means you force the shape before storing, as a warehouse does. **Schema on read** means you store raw data and apply a shape when you read it, as a lake often does. Schema on read is flexible and can become a swamp if nobody documents the raw data.

### 3. Storage

Three homes, and the differences matter.

| Home | What it is | Strength | Watch-out |
| --- | --- | --- | --- |
| Data warehouse | A structured store, modelled for reporting, often with a star schema | Trusted numbers for MIS, regulatory and finance reports | Less comfortable with raw unstructured data. Changing the model takes care |
| Data lake | A large store of raw and refined data in many formats, often files on cheap distributed storage | Keeps variety and history. Analysts can explore | Without governance it becomes a **data swamp**: nobody knows what is there or whether it is allowed |
| Lakehouse | A lake plus table features: transactions, schema enforcement, and performance, so it can serve both exploration and more trustworthy analytics | One architecture for files and tables | Still needs governance. The label does not create quality |
| Operational database | The running system's database | Exact, current transactions | Not the place to run a heavy five-year exploratory scan |

**Data mart** means a smaller warehouse area for one domain, such as cards or branches.

**Hot, warm, and cold** data: hot is used constantly and kept fast and expensive. Cold is kept cheaply because it is rarely read. Lifecycle policies move data as it ages, in line with retention rules.

### 4. Processing

Turn stored data into something usable.

- Clean names, dates, and duplicates.
- Join channel logs with product data.
- Aggregate: total failed logins by hour, average handle time by circle.
- Detect anomalies.
- Build features for a model.

Batch processing handles a big pile. Stream processing handles the flow. Many banks run both. This is the **lambda** idea: a batch path for the complete picture and a speed path for the moment. The **kappa** idea says "treat everything as a stream" to simplify. You can mention both as design choices. Lambda is the one to explain first.

### 5. Analytics and serving

The result must reach a person or a system.

- A dashboard for a circle head.
- A file for finance.
- A score returned to the payment switch.
- A feature inside the mobile app, such as a spending category view, built from governed data.

**Serving** means making the result available at the right speed and with the right permission. A dashboard and a real-time API are both serving. They serve different decisions.

**Say it in one line:** Data is sourced, ingested, stored, processed, analysed, and served. A warehouse is structured and trusted for reports. A lake holds raw variety. A swamp is a lake nobody governs.

---

## The ecosystem, name by name

An **ecosystem** is the family of tools that work together. Hadoop made this family famous. You do not need to operate them. You need to know what job each name does.

### Hadoop, the umbrella

**Apache Hadoop** is an open-source framework for storing and processing large data across clusters of ordinary machines.

A **cluster** is a group of machines that act as one system. **Open source** means the source code is available under a licence that allows use and improvement. Commercial companies package Hadoop and add support. Banks usually buy a distribution and support, not a folder of code from a website.

Hadoop's original pair:

| Piece | Job |
| --- | --- |
| HDFS | Hadoop Distributed File System. Stores files across many machines, with copies |
| MapReduce | A way to process that data in parallel: map does local work, reduce combines results |
| YARN | Yet Another Resource Negotiator. The cluster's resource manager. It decides which job gets which CPU and memory |

### HDFS, slowly

A large file is split into blocks. Blocks are spread across machines. Each block is replicated, often three copies, on different machines. If one disk dies, a copy remains. The system can also read from several disks at once, which helps volume.

This is **distributed storage**. The file looks whole to the user. Physically it is scattered on purpose.

HDFS is excellent for large files written and then read many times. It is a poor replacement for a core banking database that updates one balance thousands of times a second with strict ACID needs.

### MapReduce, slowly

Suppose you must total UPI volume by state for a year.

- **Map:** each machine totals the rows it holds, by state.
- **Shuffle:** totals for the same state are brought together.
- **Reduce:** the state totals are summed into one answer.

Work happens where the data already sits, instead of dragging every row to one computer. That is **data locality**, and it is the central trick of distributed processing.

MapReduce is reliable and can feel slow for interactive questions, because it writes intermediate results to disk. Newer engines keep more in memory.

### The names around Hadoop

| Name | One-line job |
| --- | --- |
| Hive | Lets you write SQL-like queries over data in the lake. Hive turns them into jobs. Good for batch analytics |
| Pig | An older scripting language for data flows on Hadoop. Less common in new work. Recognise the name |
| HBase | A NoSQL wide-column database on top of HDFS, for fast reads and writes of large sparse tables. Inspired by Google Bigtable |
| Sqoop | Moves data between relational databases and Hadoop |
| Flume | Collects and moves large amounts of log data into Hadoop |
| Oozie | Schedules Hadoop jobs. You may never see the name in a paper. It is the workflow clock |
| ZooKeeper | A coordinator. It keeps configuration and who-is-the-leader information for distributed systems |
| Kafka | A distributed log for streaming events between producers and consumers. Central to modern real-time pipelines. Not only a Hadoop tool. Often the backbone |
| Spark | A fast distributed processing engine. In-memory where it can. Handles batch, streaming, SQL, and machine learning |
| Flink | Another strong stream-processing engine. Mention beside Spark Streaming as a modern alternative |

### Spark, because it is the engine you will actually meet

**Apache Spark** can read from HDFS, from a lake, or from Kafka. It is popular because many operations stay in memory, so repeated analysis is faster than classic MapReduce.

Pieces of the Spark family:

| Piece | Job |
| --- | --- |
| Spark SQL | Query structured data with SQL |
| Spark Streaming, and Structured Streaming | Process a stream as small batches or as a continuous table |
| MLlib | Spark's machine-learning library |
| DataFrame | A table-like structure inside Spark. The word you will see in notebooks |

### NoSQL, the storage cousins

**NoSQL** means databases that are not only classic relational tables. The name is historical. Many of them now speak a query language too.

| Type | Picture | Example names | Bank-shaped use |
| --- | --- | --- | --- |
| Key-value | A dictionary. A key finds a value very fast | Redis, Dynamo-style stores | A session, a cache, a hot customer profile |
| Document | A JSON-like document | MongoDB | A flexible case file or a product catalogue |
| Wide-column | Rows with very many optional columns, spread across machines | HBase, Cassandra | High-volume event or interaction histories |
| Graph | Nodes and relationships | Neo4j | "Who is connected to whom" in a fraud ring. Use with privacy limits |

NoSQL does not mean "no rules" and does not mean "better than SQL". Relational databases remain right for the ledger. NoSQL is right for some large, flexible, or extremely fast access patterns.

Many NoSQL systems offer **BASE** thinking rather than strict ACID: Basically Available, Soft state, Eventually consistent. **Eventual consistency** means all copies will agree, but not necessarily in the same instant. That can be acceptable for a product catalogue. It is not acceptable for "did this debit happen exactly once?" unless the design adds very careful controls.

### Cloud cousins of the same jobs

On a cloud, the jobs stay the same and the product names change: object storage instead of HDFS, a managed Kafka, a managed Spark, a cloud warehouse such as Snowflake, BigQuery, or Redshift. If you understand the job, you can recognise a new brand.

**Say it in one line:** HDFS stores, MapReduce and Spark process, YARN schedules resources, Hive queries, Kafka moves events, Sqoop and Flume ingest, and HBase serves large sparse tables. Spark is the modern processing engine to emphasise.

---

## Distributed storage and processing

### Why "distributed"?

One machine has a ceiling: one set of disks, one CPU, and one failure that stops everything. Distribution uses many machines.

Two gains:

- **Scale.** Add machines to add space and processing power.
- **Resilience.** Copies and restarts mean one disk is not a crisis.

Two costs:

- **Complexity.** Parts fail every week in a large cluster. The software must expect failure.
- **Coordination.** Machines must agree, or you must design so they do not need to agree instantly.

### How distributed storage stays honest enough

- **Replication.** Several copies of each block or record.
- **Rack awareness.** Copies are placed on different racks, so one power failure does not remove all copies.
- **Checksums.** A fingerprint detects silent corruption.
- **Rebalancing.** When a new machine arrives, data is spread to use it.

### How distributed processing stays honest enough

- The job is split into tasks.
- A failed task is retried on another machine.
- The manager tracks progress.
- If the input is unchanged, a well-written task produces the same output when retried. That property makes retries safe.

### A CAP reminder, only at the depth a manager needs

The **CAP** idea says that in a distributed system, during a network split, you trade off consistency and availability. You cannot always have every copy instantly agree and also answer every call as if nothing is wrong.

Practical meaning:

- For a payment ledger, lean toward consistency.
- For a non-critical dashboard, you may show data a minute old and stay available.

You will not be asked to prove CAP. You may be asked what eventual consistency means. Answer: replicas can briefly differ, and they converge.

### Batch, stream, and micro-batch

| Word | Meaning |
| --- | --- |
| Batch | Process a bounded set: "yesterday's file" |
| Micro-batch | Process very small frequent batches, such as every few seconds. Spark Structured Streaming often feels like this |
| True streaming | Process each event as it arrives. Flink is known for this |
| Window | A time slice of a stream: "count failures in each minute" |
| Watermark | A way to handle late events. If a transaction event arrives two minutes late, the window still needs a rule |

### OLTP and OLAP

| | OLTP | OLAP |
| --- | --- | --- |
| Expansion | Online transaction processing | Online analytical processing |
| Job | Run the business now | Understand the business |
| Operations | Many small updates | Fewer, larger reads and aggregates |
| Example | Post a deposit | Compare deposit growth by product and region for five years |
| Typical home | Relational core database | Warehouse, mart, or lakehouse |

Big data work is mostly on the OLAP side and on exploration. It should not replace OLTP for the ledger.

**Say it in one line:** Distributed storage spreads copies of data across machines. Distributed processing sends the work to those machines. The ledger stays strongly consistent. Analytics can tolerate a little delay when the business can.

---

## Analytics

Analytics means turning data into a decision.

### Four kinds, in rising ambition

| Kind | Question | Bank example |
| --- | --- | --- |
| Descriptive | What happened? | Failed logins rose 20 percent yesterday in one circle |
| Diagnostic | Why did it happen? | A release at 4 pm coincided with the rise, and the errors are timeout errors from one service |
| Predictive | What is likely to happen? | These accounts are more likely to become dormant. This transaction has a higher fraud probability |
| Prescriptive | What should we do? | Offer this service call, or step up authentication, within policy |

A dashboard is usually descriptive. A root-cause study is diagnostic. A score is predictive. A recommended next action is prescriptive. Prescriptive systems can affect customers, so they need stronger governance than a chart.

### How the work is done

| Word | Meaning |
| --- | --- |
| BI, business intelligence | Reports and dashboards for known questions. The monthly business review pack |
| Ad hoc analysis | A new question asked by an analyst |
| Data mining | Searching data for patterns that were not specified in advance |
| Machine learning | Models that learn those patterns and score new cases |
| Segmentation | Grouping customers by behaviour or need, so service and offers are relevant and fair |
| MIS | Management information system. The regular numbers managers use |
| KPI | Key performance indicator. A chosen number, such as digital adoption or complaint turnaround |
| Visualisation | Charts and maps. They must say the unit, the period, and the source |

### A careful fraud example

1. Descriptive: card declines increased after 11 pm.
2. Diagnostic: most are from three merchant categories and new devices.
3. Predictive: a model scores each authorisation.
4. Prescriptive: above a threshold, the switch asks for a step-up, or declines, according to a policy a person approved.

The model did not invent the policy. Management set the threshold, the customer impact, and the grievance path. Analytics supplied evidence and a score.

### Metrics that mislead

- Averages hide the unhappy tail. Look at volumes and at the worst periods.
- A percentage without a base is incomplete. "100 percent growth" from 2 to 4 customers is not a strategy.
- Correlation is not a cause. Two lines rising together need investigation.
- A model trained on last year's fraud misses this year's new pattern unless it is monitored and refreshed.
- Historical bias in data becomes future bias in scores.

**Say it in one line:** Descriptive tells what happened, diagnostic tells why, predictive tells what may happen, and prescriptive suggests an action. The higher you climb, the more you need a human policy around the number.

---

## Data governance

**Data governance** is the system of decision rights, standards, and controls that makes data fit to use and safe to hold.

Without it, a lake is a swamp, a report argues with the next report, and a customer's data travels further than the customer or the law allows.

### The roles

| Role | Duty |
| --- | --- |
| Data owner | A business leader accountable for a domain, such as deposits data. Decides purpose and access policy |
| Data steward | The person who looks after definitions, quality rules, and issues day to day |
| Data custodian | The technology team that stores, secures, and backs up the data |
| Consumer | The user of a report or a model. Responsible for using it within the purpose |

Ownership sitting only with IT is a common failure. IT can custody a database. The business must own the meaning of "active account" or "complaint resolved".

### The practices

**Catalogue and dictionary**

A **data dictionary** defines fields: "available balance means withdrawable amount after holds, as of the core posting time". A **data catalogue** is the index of datasets: what exists, who owns it, where it is, and how sensitive it is. People cannot govern what they cannot find.

**Quality**

Quality dimensions worth listing:

| Dimension | Question |
| --- | --- |
| Accuracy | Does it match the real-world fact? |
| Completeness | Are required fields present? |
| Timeliness | Is it fresh enough for this use? |
| Consistency | Do two systems agree where they should? |
| Uniqueness | Are there duplicate customers or duplicate transactions? |
| Validity | Does the value belong to the allowed set? A date is a date. A state code exists |
| Integrity | Do relationships hold? Every transaction points at a real account |

Measure quality. Publish the defects. Fix them at the source when you can. Fixing only the report hides the wound.

**Lineage**

**Data lineage** means the path from source to report. "This NPA figure came from these extracts, through this job, with this rule." When a number is challenged, lineage is how you answer. Regulators and auditors ask this in substance even when they do not use the word.

**Classification and access**

Label data: public, internal, confidential, customer secret. Access follows the label and the purpose. Analytics copies deserve the same seriousness as production, because they often hold more history in one place.

**Retention and disposal**

Keep data for the period policy and law require. Do not keep it forever by default. Disposal includes copies, backups, and the lake, not only the main table.

**Privacy**

Personal data is collected for a purpose, used for that purpose, and shared on a lawful basis. India's Digital Personal Data Protection Act, 2023, is the broad frame: consent or another lawful ground, purpose limitation, security safeguards, and rights of the individual. Sector rules from the Reserve Bank still apply to banking data.

Practical habits:

- Mask or tokenise account numbers and PANs in analytics where the raw value is not needed.
- Prefer aggregated data when the question is about a region, not a person.
- Do not join datasets just because you technically can, if the purpose is not approved.
- A vendor who builds a model must be bound by contract on where data sits and whether it trains their other models.

**Master data**

**Master data** is the shared core: customer, product, branch, employee. **MDM**, master data management, tries to make one trustworthy view. A customer with three customer ids and two dates of birth will break both service and compliance. Big data does not solve that by itself. MDM and deduplication do.

**Reference data**

**Reference data** is the standard list: country codes, currency codes, branch codes, product codes. Govern the list in one place.

**Metadata**

**Metadata** is data about data: when it was loaded, who owns it, what the fields mean, what quality score it has. A lake without metadata is a room of unlabelled boxes.

### Governance of models built on big data

A fraud model or a collection model inherits every defect of its data. Governance includes:

- known training sources and time range,
- approval to use those sources,
- a check that the model does not use forbidden attributes,
- monitoring for drift,
- a documented reason for customer-affecting actions,
- and a path to correct a wrong outcome.

This links to the generative-AI chapter. The data underneath is the same governance problem whether the output is a score or a paragraph.

### Security wrapped around the platform

- Authentication of every analyst and every job.
- Least privilege. An exploration sandbox is not production write access.
- Encryption at rest and in transit.
- Audit of queries against sensitive data.
- Separation of environments.
- Controlled extraction. Downloading a crore-row customer file to a laptop is a governance failure even if the lake is secure.

**Say it in one line:** Governance names an owner, defines the data, measures quality, traces lineage, limits use, and keeps personal data inside purpose and law. Tools do not replace that.

---

## A full bank picture you can retell

The bank wants to reduce sudden card fraud without delaying every genuine swipe.

1. **Volume and velocity.** Authorisation events are numerous and fast.
2. **Variety.** The decision may look at the transaction, the device, and recent history, which arrive as structured fields and semi-structured events.
3. **Ingestion.** Kafka carries the events.
4. **Storage.** Recent events sit in a fast store. History sits in the lake.
5. **Processing.** A stream job computes features such as "number of swipes in the last ten minutes". Spark or Flink does this work across machines.
6. **Veracity.** Missing device ids are flagged. Duplicate event ids are dropped so one swipe is not counted twice.
7. **Analytics.** A supervised model, trained on labelled past fraud, returns a score.
8. **Value.** The score supports a policy: below a line, approve; in the middle, step up; above a line, decline and alert.
9. **Governance.** The model has an owner. Training data was approved. Customers have a dispute path. Retention of device data is defined. An analyst cannot browse card numbers for curiosity.
10. **The ledger.** The actual debit, if approved, is still posted by the card and core systems with transactional integrity. The big-data platform advised. It did not become the system of record.

---

## Big-data mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "The 5Vs are volume, velocity, variety, virtualisation, and VPN" | The core five are volume, velocity, variety, veracity, and value |
| "A data lake is a data warehouse" | A warehouse is structured for trusted reporting. A lake stores raw variety. A lakehouse tries to offer both disciplines |
| "HDFS replaces the core database" | HDFS is distributed file storage for large analytical data. The ledger needs a transactional database |
| "MapReduce is a storage system" | MapReduce processes. HDFS stores. YARN manages resources |
| "NoSQL means no queries and no structure" | It means a non-relational design. Each type has its own structure |
| "Eventual consistency is fine for account balances during a debit" | Eventual consistency can suit catalogues and some caches. A debit needs a consistent transaction |
| "Analytics copies can be casual because they are not production" | They often hold more customer history than one operational screen. Govern them |
| "ETL and ingestion are decoration" | They are where quality, duplication, and privacy are won or lost |

## Remember these lines

- 5Vs: volume, velocity, variety, veracity, value.
- The path is source, ingest, store, process, analyse, serve, with governance around all of it.
- Warehouse for trusted structured reporting. Lake for raw variety. Swamp if the lake is ungoverned.
- HDFS stores in blocks with copies. MapReduce and Spark process in parallel. Kafka streams events. Hive queries. Spark is the modern engine to highlight.
- OLTP runs today's transactions. OLAP studies them.
- Descriptive, diagnostic, predictive, prescriptive: what happened, why, what next, what to do.
- Governance: owner, steward, dictionary, quality, lineage, classification, retention, privacy, and least privilege.

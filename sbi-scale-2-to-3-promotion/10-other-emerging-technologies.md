# 10. Other Emerging Technologies

The syllabus line is: basics of Blockchain, IoT, Cloud-native platforms, Edge Computing, RPA, and Quantum Computing.

Each section answers four things: what it is, how it works in simple steps, where a bank might use it, and where a manager should be careful.

---

# A. Blockchain

## The one story

Today, when two banks need a shared fact, they often each keep their own record and then reconcile. Reconciliation means comparing the two books and fixing differences. It works, and it takes time.

A **blockchain** is a shared ledger. Several parties hold copies. A new entry is added only when the network accepts it, and old entries are practically impossible to edit quietly because each entry is tied to the one before it.

**Say it in one line:** Blockchain is a shared, append-only ledger protected by cryptography and agreement among participants, so many parties can rely on one history without a single person silently rewriting it.

## Words, one by one

| Word | Simple meaning |
| --- | --- |
| Ledger | A book of records |
| Distributed ledger | A ledger copied across many participants. DLT, distributed ledger technology, is the wider family. Blockchain is one kind of DLT |
| Block | A page of the ledger. It contains a batch of transactions |
| Chain | The pages are linked in order. Each block points to the previous block |
| Hash | A fingerprint of data. Change one letter and the fingerprint changes completely. You cannot rebuild the original data from the fingerprint alone |
| Previous-hash link | Each block stores the hash of the block before it. Change an old block and every later hash stops matching |
| Transaction | One recorded event, such as "this document was issued" or "this asset moved" |
| Node | One computer that holds a copy and follows the rules |
| Consensus | The rule by which nodes agree that a new block is valid |
| Immutable | Practically unchangeable. You correct a mistake by adding a new entry, not by erasing the old one |
| Smart contract | A program stored on the chain that runs when conditions are met. It is neither smart in a human sense nor a legal contract by itself |
| Wallet | A place that holds keys which control a blockchain identity. In public crypto networks it also relates to coins. In a bank context, think "key holder", not "unregulated savings account" |
| Public blockchain | Anyone can join. Bitcoin and Ethereum are examples. Open, and slower to govern |
| Private or permissioned blockchain | Only approved organisations run nodes. This is the form banks actually consider |
| Consortium blockchain | A permissioned chain run by a group, such as several banks |
| On-chain | Recorded on the ledger |
| Off-chain | Kept outside, with a reference on the ledger. Used for bulky or private data |
| Token | A digital representation of a right or an asset on a chain. It can represent money, a bond, or a claim. The legal meaning depends on the design and the regulation, not on the word |
| Gas or fee | A charge to run a transaction on some public chains. Permissioned bank chains may not use a public fee model |

## How a block gets added

1. A participant submits a transaction, signed with their private key. The signature proves it came from the holder of that key.
2. Nodes check the rules: signature valid, format valid, not a double use of the same reference if that rule applies.
3. Valid transactions are gathered into a candidate block.
4. Nodes run the consensus rule until they agree.
5. The new block stores the previous block's hash and is copied to the participants.
6. Later blocks build on it. Changing history would mean redoing all of those links and convincing the network. In a well-run chain, that does not happen quietly.

## Consensus, only as deep as the exam needs

| Method | Plain idea | Where it fits |
| --- | --- | --- |
| Proof of Work | Many computers race to solve a hard puzzle. The winner proposes the block. It uses a great deal of electricity | Public networks such as Bitcoin. Not a bank's private ledger |
| Proof of Stake | Participants put value at stake and are chosen to validate. Misbehaviour can cost the stake | Some public networks |
| Practical Byzantine Fault Tolerance and voting methods | Known members vote. The block is accepted when enough known members agree | Permissioned networks. This is the family to associate with bank consortia |

**Byzantine fault** means a participant may fail or lie, and the system still reaches a correct agreement. The name comes from a thought experiment about generals. You only need the idea: consensus is designed to survive some dishonest or broken nodes.

## What "cryptographic" gives you

- **Hash** links the pages and detects tampering.
- **Digital signature** shows which approved participant submitted the transaction.
- **Merkle tree** is a hash-of-hashes inside a block, so you can prove one transaction belonged in the block without sending every transaction. Know the purpose, not the drawing.

## Smart contracts, with the romance removed

A smart contract is code: "if the bill of lading reference and the payment reference both arrive, mark the trade finance step complete and notify both parties."

It will do exactly what the code says, including the bug. It does not understand fairness. It should be tested, limited, and paired with a legal agreement off the chain. Upgrade and dispute processes must exist, because immutability makes mistakes stick.

## Where a bank may use it

| Use | What is shared | Why a shared ledger helps |
| --- | --- | --- |
| Trade finance | Letters of credit, invoices, shipment milestones among bank, buyer, seller, and shipper | Everyone sees the same milestone. Less paper chase |
| Cross-border payments experiments | Payment status among participating institutions | Faster agreement on status. The legal money movement still follows payment-system rules |
| Syndicated loans | The common record of shares and payments among lender banks | One view of the facility |
| KYC utility, in a careful design | A proof that KYC was done, or a shared update, with the customer's consent | The customer is not asked for the same document in a wasteful way. Raw identity documents may stay off-chain |
| Bond or asset record | Issuance and ownership of a permitted instrument | Fewer reconciliation breaks |

The Reserve Bank of India and the broader official sector have explored distributed ledgers, including wholesale and retail central-bank digital currency pilots. A **CBDC** is a digital form of sovereign currency, issued by the central bank. It may use distributed-ledger ideas and it is not the same thing as a private cryptocurrency. If you mention it, keep that difference sharp.

## Where a manager should be careful

- **Privacy.** A ledger shared with other banks must not become a window into customers who did not consent. Prefer permissioned chains and off-chain private data.
- **Cryptocurrency hype.** A bank's interest in DLT is not a reason to treat speculative coins as a product unless regulation expressly allows that activity.
- **"Immutable" is not "correct".** A wrong entry stays visible. You need a governed way to post a reversing entry.
- **Throughput.** Public chains can be slow and costly. A core banking ledger that posts ordinary deposits is not waiting to move onto a public chain.
- **Key custody.** Lose the private key and you lose the ability to authorise. Steal the key and the thief can authorise. Key management is the control.
- **Legal finality.** Technology agreement is not always legal settlement finality. Payment and securities law still decide when a transfer is final.
- **Vendor and consortium governance.** Who can upgrade the code, who can see what, and how a member exits.

**Say it in one line:** Blockchain is a tamper-evident shared ledger. Banks look at permissioned versions for multi-party processes such as trade finance. It does not replace the core ledger for ordinary deposits, and it does not make a record true just because it is unchangeable.

---

# B. Internet of Things

## The one story

A computer with a person at a keyboard is not the only source of data. A machine can notice something in the physical world and send that fact onward: the ATM door opened, the cash cassette is low, the server-room temperature rose, the branch inverter switched on.

**IoT**, the Internet of Things, means physical objects with sensors, software, and a connection, so they can collect and exchange data.

**Say it in one line:** IoT is ordinary objects made able to sense, connect, and report, so the bank can know the state of places and machines without someone inspecting each one.

## The parts

| Part | What it does | Example |
| --- | --- | --- |
| Sensor | Measures something | Temperature, door open, vibration, cash level, power status |
| Device or "thing" | The object that holds the sensor and a small computer | An ATM, a smart meter, a badge reader |
| Actuator | Does a physical action, not only sensing | A lock, an alarm siren, a switch |
| Connectivity | The path the data travels | Ethernet, Wi-Fi, mobile data, sometimes low-power long-range radio |
| Gateway | A local collector that receives data from many small devices and forwards it securely | A branch gateway that gathers ATM and premises alerts |
| Device identity | A credential so the network knows this is a genuine device | A certificate in the ATM |
| Platform | The central software that stores events, shows dashboards, and raises alerts | An operations console |
| Application | The business use of the data | "Dispatch cash replenishment" or "raise a facilities ticket" |

A sensor does not, by itself, make a decision about a customer's money. It informs operations.

## How a reading becomes an action

1. The sensor measures.
2. The device sends a small message: device id, time, reading.
3. The gateway checks identity and forwards it.
4. The platform compares the reading with a rule. Temperature above a set point is an exception.
5. An alert goes to the team, or a ticket is created.
6. A person or a controlled actuator responds.
7. The event is logged.

**Edge** often sits in this path. The gateway may decide simple things locally, such as "door forced, ring the local alarm now", and send the history to the centre later. That link is in the edge section.

## Bank uses that are realistic

| Place | What is sensed | Value |
| --- | --- | --- |
| ATM | Cash level, card-reader health, vibration, door, power | Refill before the ATM goes cash-out. Detect tampering faster |
| Branch premises | Intrusion, fire, temperature, water leak | Protect people, records, and cash |
| Data centre | Temperature, humidity, power, water leak | Prevent outages. These sensors already exist in mature data centres. IoT is the connected, managed form |
| Cash and document logistics | Location and seal status of a van or a box, within the law and policy | Better chain of custody |
| Energy | Meter readings | Lower cost and earlier warning of waste |
| Customer service devices | Queue tokens and kiosk health | Open another counter when the queue grows. This is operations data, not surveillance for its own sake |

Be conservative about wearable or camera analytics. A connected camera can be an IoT device. Using it to watch staff or customers raises privacy, dignity, and policy questions. The existence of a sensor is not an approval to collect everything.

## Risks

| Risk | Why it is sharp in IoT |
| --- | --- |
| Weak device security | Small devices ship with default passwords and may never be patched |
| Huge number of endpoints | Every device is something an attacker can try |
| Physical access | An ATM is on a street. An attacker can touch the device, unlike a server in a hall |
| Bogus data | A compromised sensor can report "temperature normal" during a fire, or "vault closed" when it is open |
| Privacy | Location and video can identify people |
| Availability | A dead SIM card or a dead gateway creates a blind spot |
| Update | You need a way to update firmware safely. Firmware is the device's built-in software |

Controls: unique credentials, no default passwords, network segmentation so an ATM cannot browse the core database, encryption, a device inventory, patching, and alerts when a device goes silent. Silence is also an event.

**Say it in one line:** IoT connects sensors and machines so the bank can run ATMs, branches, and data centres with earlier warning. Each device is a computer that must be identified, patched, and kept out of the wrong network.

---

# C. Cloud-native platforms

## The one story

Older systems were built as one large application on one carefully hand-built server. Changing it meant a large release and a hope that the server's setup matched test.

**Cloud-native** means building and running applications so they expect a cloud environment: they come in small pieces, start quickly, scale by adding copies, and are deployed by automation.

Cloud-native is an approach. It can run on a public cloud, a private cloud, or a bank data centre that offers the same platform services.

**Say it in one line:** Cloud-native is a way of building applications as small, containerised, automatically managed services that can scale and recover without hand-built servers.

## The vocabulary

| Word | Simple meaning |
| --- | --- |
| Container | A standard package that holds the application and the libraries it needs. It starts quickly and runs the same in every environment. Docker popularised the format |
| Image | The built package. A running container is an image that has been started |
| Orchestration | Software that places, starts, restarts, and scales many containers. Kubernetes is the standard |
| Kubernetes | The control system. You tell it "keep five copies of the payments service running". It works to make that true |
| Pod | In Kubernetes, the smallest running unit, usually one main container |
| Service | A stable name and address in front of changing pods |
| Cluster | The group of machines Kubernetes manages |
| Namespace | A logical partition inside a cluster, useful to separate teams or environments |
| Microservice | A small service owning one capability. Cloud-native often uses this shape. A container can also hold a well-built monolith. Containers and microservices are related and not identical |
| Declarative | You describe the desired end state, and the platform tries to reach it. You do not log in and click through steps each time |
| Immutable infrastructure | You do not patch a running server by hand. You replace it with a new image |
| CI/CD | Automated build, test, and release |
| DevOps | The culture and practice of building and running as one flow, with shared responsibility for production health |
| DevSecOps | The same flow with security checks inside it, not as a late surprise |
| API-first | The service contract is designed clearly, because other services depend on it |
| Observability | Logs, metrics, and traces so you can see what a distributed system is doing |
| Service mesh | A layer that handles service-to-service security, retries, and traffic policy, so every service does not reinvent them. Istio and Linkerd are names. Know the job |
| Elasticity | Add and remove copies with demand |
| Twelve-factor ideas | A set of habits: config outside the code, clear dependencies, logs as streams, stateless processes. You can mention "twelve-factor" as the classic cloud-native checklist |

## How the pieces cooperate

1. Developers build a service.
2. The pipeline creates an image and runs tests and security scans.
3. Kubernetes pulls the image and starts the desired number of copies.
4. A service name stays constant while copies come and go.
5. The API gateway sends customer traffic in.
6. If a copy crashes, Kubernetes starts another.
7. Metrics show whether you need more copies.
8. A new release rolls out copy by copy, or as a canary.

## What a bank gains, and what it must not lose

Gains:

- Faster, more repeatable releases.
- Better use of machines.
- Failure of one copy is ordinary, not a crisis.
- Test and production resemble each other more closely.

Duties that remain:

- The platform itself must be highly available. Kubernetes is not a magic shield.
- Data still needs backups, transactions, and ownership. A container disappearing must not lose the ledger.
- Security of the cluster, the registry of images, and the secrets is now critical infrastructure.
- More moving parts demand stronger observability and clearer ownership.
- Regulated change management still applies. Automation speeds the path. It does not remove approval where policy requires approval.

**CNCF**, the Cloud Native Computing Foundation, hosts Kubernetes and many related projects. The name is enough.

**Say it in one line:** Containers package the application. Kubernetes keeps the right number running. CI/CD releases them. Observability shows their health. That combination is a cloud-native platform.

---

# D. Edge computing

## The one story

If every decision travels to a data centre hundreds of kilometres away, some actions feel late. An ATM that must decide whether a vibration pattern looks like an attack, or a branch gateway that must alarm on a forced door, may need a local decision in milliseconds.

**Edge computing** means doing computation close to where the data is created, at the edge of the network, instead of sending everything to the central cloud first.

**Say it in one line:** The edge is computing near the device or the branch. The centre still holds the master data and the harder, wider analysis.

## A map of places

| Place | Where it is | What runs there |
| --- | --- | --- |
| Device edge | Inside the ATM or sensor | Very small, immediate checks |
| Branch or premises edge | A small computer in the branch | Local alarms, local buffering if the network drops, kiosk support |
| Network edge | Near a telecom or provider node | Content caches, some security filtering |
| Regional edge | A smaller data centre closer than the national centre | A regional cache or a local processing zone |
| Central cloud or data centre | The main site | Core ledger, enterprise analytics, master models |

Edge is not a rejection of cloud. A common pattern is: decide urgent things locally, send summaries inward, train models centrally, and push the approved model back out to the edge.

## Why bother

| Reason | Meaning |
| --- | --- |
| Latency | Less distance, faster reaction. Latency is waiting time |
| Bandwidth | A camera should not send raw video all day if only the "door forced" event matters |
| Resilience | The branch can still alarm and buffer if the WAN link fails. WAN means the wide network between branch and centre |
| Privacy | Some raw data can be processed locally and only an alert leaves the building |
| Cost | Less unnecessary traffic |

## A careful ATM story

1. Sensors in the ATM notice repeated vibration and an open panel.
2. The ATM or the branch edge device raises a local alarm immediately.
3. It also stores the event.
4. When the link is up, it sends the alert and a short clip or a summary, according to policy, to the security operations centre.
5. The centre correlates this ATM with others.
6. The cash-out decision for a customer is still made by the payment and card systems, not by the vibration sensor.

The edge handled urgency. The centre handled the wider picture. The ledger handled the money.

## Risks

- Many edge boxes mean many places to patch and to physically protect.
- A local box can be stolen. Disks must be encrypted, and secrets must not be stored casually.
- Versions can drift: one branch runs last year's software. Central management of edge fleets matters.
- Local decisions must have safe limits. An edge device should not be able to invent a credit limit.
- Data that stays at the edge still needs retention and access rules.

**Say it in one line:** Use the edge for low delay, local survival, and less useless traffic. Use the centre for the ledger, the master model, and the enterprise view.

---

# E. RPA — Robotic Process Automation

## The one story

Some office work is repetitive and rule-bound: open a screen, copy a reference number, paste it into another screen, check a total, and type "matched" or "exception". A person can do it. They will also get tired and make copying mistakes.

**RPA** uses software robots, called **bots**, to perform those steps on existing systems, often through the same screens a person uses.

The robot does not walk or hold a pen. "Robotic" means it mimics a user's actions or calls the system's interface.

**Say it in one line:** RPA is a software bot that repeats clear, rule-based computer tasks, especially across old systems that do not offer a clean API.

## How a bot works

1. A process expert writes the steps and the rules, including what to do when something is missing.
2. The bot is given its own identity, not a staff member's password.
3. It logs in, reads fields, applies the rules, and writes results.
4. Exceptions that do not match the rules go to a human queue.
5. Every run is logged.

Two styles:

| Style | How it runs | Example |
| --- | --- | --- |
| Attended | Beside a person, started by them, on their workstation, helping one case | A contact-centre bot that fetches details while the agent talks, within policy |
| Unattended | On a server, on a schedule or a queue, without a person clicking start | Overnight reconciliation of a payment file |

## Good candidates and bad candidates

A process is a good RPA candidate when:

- the rules are stable and explicit,
- the volume is high,
- the steps are digital,
- exceptions are a minority,
- and the source screens do not change every week.

A process is a bad candidate when:

- judgement is the main work,
- the rules change constantly,
- the data is messy unstructured text and nobody has checked extraction quality,
- or an API could do the same job more reliably. An API is usually a better long-term integration than a bot pretending to be a user. RPA shines when the old system has no API and replacing it is not near.

## Bank examples

| Process | What the bot does | What a person still does |
| --- | --- | --- |
| Reconciliation | Matches two files and marks breaks | Investigates the breaks |
| KYC document chase | Checks whether mandatory documents are present and in date, against a checklist | Decides doubtful cases |
| Account-opening data entry from an already verified source | Copies approved fields into the system of record | Owns the customer relationship and the exception queue |
| Report distribution | Pulls a standard report and sends it to the right desk | Interprets the report |
| Standing instructions and mandate checks | Compares mandate fields against a checklist | Handles mismatches |
| NPA and limit monitoring support | Gathers data from several screens into one case file | Makes the credit judgement |

RPA can sit beside document AI. The model reads a scanned form. The bot posts the accepted fields. Low-confidence fields never go straight through.

## Controls

- The bot has a unique id. Its actions are auditable. "The robot did it" is not an acceptable unknown.
- Least privilege. A reconciliation bot does not need the power to create users.
- Password vault. Credentials are not written in the script in plain text.
- Change control. If a screen label changes, the bot can mis-type into the wrong field at high speed. High speed multiplies a mistake.
- Exception handling and a human queue.
- Volume and runtime monitoring. A bot that silently stopped is a process failure.
- Business ownership. IT runs the platform. The business owns the process and the rules.
- Customers should not be told a decision was "made by a robot" as a way to avoid a grievance path. The bank made the decision.

**Hyperautomation** is a buzzword for combining RPA, workflows, rules, and AI. If you use it, explain the pieces. Do not use it as a fog.

**Say it in one line:** RPA automates stable, high-volume, rule-based screen work. Give every bot an identity, a limit, a log, and a human path for exceptions. Prefer an API when one exists.

---

# F. Quantum computing

## The one story

A normal computer bit is either 0 or 1. A quantum computer uses **qubits**, which follow quantum physics. A qubit can be in a combination of 0 and 1 while it is computing. A set of qubits can represent many combinations at once, in a way classical bits cannot. For a few kinds of problems, this lets a quantum computer explore a huge space far more efficiently.

It does not make every banking program faster. It will not replace the branch server next quarter. It matters now because some of the cryptography that protects today's data could, in the future, be broken by a large enough quantum computer, and because data stolen today could be stored and decrypted later.

**Say it in one line:** A quantum computer uses qubits to solve certain very hard problems much faster. Banks should care now about the future threat to encryption, even though useful large machines are not yet ordinary infrastructure.

## Classical computing, as the baseline

| Word | Meaning |
| --- | --- |
| Bit | The smallest classical unit. It is 0 or 1 |
| Byte | Eight bits |
| Classical computer | Every computer you use today: laptop, server, phone |
| Algorithm | A defined method for solving a problem |

Classical computers are excellent at ledgers, web serving, and almost all bank work. Quantum computers are not a new brand of laptop.

## Quantum words, in pictures

**Qubit.** A quantum bit. During computation it can be in **superposition**, meaning a weighted combination of 0 and 1. When you measure it, you get a definite 0 or 1. Superposition is not "it is both, so we skip the work". It is a physical state that algorithms use carefully before measurement.

**Superposition.** The qubit has not been forced into a single classical answer yet. A coin spinning in the air is a classroom picture. It is only a picture. A qubit is not a coin, and the picture breaks if you push it too far. Use it to remember "not yet a single settled bit".

**Entanglement.** Two qubits can be linked so that measuring one tells you something about the other, even when they are separated. This is a resource quantum algorithms use. It is not a communication method that sends a message faster than light, and it is not a mystical link between bank accounts.

**Interference.** Quantum algorithms arrange states so wrong answers cancel out and right answers become more likely. This is why a quantum algorithm is a clever design, not merely "try all answers in parallel" in the casual sense.

**Measurement.** Reading the qubit collapses the computation to classical bits. You often run a quantum program many times and look at the distribution of answers.

**Decoherence.** The qubit loses its delicate state because of noise, heat, or vibration. This is why quantum chips are hard to build and are kept in extreme conditions. Results today are limited by noise. The machines are called **NISQ**, noisy intermediate-scale quantum, in current research writing.

**Quantum supremacy or quantum advantage.** A demonstration that a quantum machine did a task beyond practical classical reach. Headline demonstrations may be narrow tasks with no banking use. Advantage on a useful problem is the milestone that matters. Treat press claims with care.

## What quantum computers are good at, and what they are not

They are promising for certain problems:

- Simulating molecules and materials.
- Some optimisation problems.
- Some kinds of search and sampling.
- **Factoring large numbers and solving the discrete-logarithm problem**, through **Shor's algorithm**, if the machine becomes large and stable enough. This is the banking-relevant point, because much public-key cryptography rests on those hard problems.
- **Grover's algorithm** speeds up unstructured search, roughly by a square-root factor. It weakens symmetric encryption somewhat, which is addressed by using longer keys. It is a different, milder effect than Shor's algorithm.

They are not good at, or not needed for:

- Posting a cash deposit.
- Hosting a website.
- Replacing a data warehouse.
- Making AI automatically wise.

A likely future is **hybrid**: a classical computer does the bank's normal work and calls a quantum service only for a special problem, if that ever becomes useful and economical.

## The risk that matters now: harvest now, decrypt later

Much of today's internet and banking security uses public-key cryptography:

| Method | Role today | Quantum concern |
| --- | --- | --- |
| RSA | Certificates, signatures, key exchange in older systems | Shor's algorithm would break it if a large fault-tolerant quantum computer exists |
| ECC, elliptic-curve cryptography | Smaller keys, used widely in modern TLS and signatures | Also vulnerable to a large quantum computer |
| AES | Symmetric encryption of data | Grover's algorithm suggests using sufficient key length, such as AES-256 for long-lived secrets. AES is not broken in the same way as RSA |
| SHA-256 and the SHA-2 family | Fingerprints and integrity | Longer hashes remain the practical response. Not the same cliff as RSA |
| Diffie-Hellman key exchange | Agreeing a shared secret | In the same danger family as RSA and ECC |

**Harvest now, decrypt later** means an attacker records encrypted traffic today and waits until a future quantum computer can decrypt it. Data that must stay secret for many years is already exposed to this waiting game. Long-lived customer data, authentication secrets, and anything with a long confidentiality life deserve attention first.

This does not mean today's TLS is pointless. It means planning cannot wait until the machine is in a newspaper.

## Post-quantum cryptography

**Post-quantum cryptography (PQC)** means classical algorithms designed to resist known quantum attacks. They run on today's computers. They are not quantum computers.

Standards bodies, especially NIST in the United States, have been selecting PQC algorithms for key agreement and digital signatures. Names you might see in a technical note include CRYSTALS-Kyber for key establishment and CRYSTALS-Dilithium, FALCON, and SPHINCS+ for signatures. Product names evolve. The exam point is the category: replace vulnerable public-key methods with standardised post-quantum methods, in a planned way.

**Crypto-agility** means the bank can change algorithms without rebuilding every system. Certificates, HSMs, ATMs, mobile apps, and partner links all have to move. An HSM is a hardware security module, a locked box that holds keys. Crypto-agility is an architecture property.

A sensible management stance:

1. Inventory where RSA and elliptic curves are used: TLS, VPN, email, code signing, payment interfaces, device certificates.
2. Mark data that must remain confidential for a long time.
3. Prefer longer symmetric keys for that data.
4. Watch standardised PQC and vendor support. Do not invent a private algorithm.
5. Plan a migration. Test, because new keys and certificates are larger and can break old devices.
6. Beware "quantum" products that are marketing. Quantum key distribution, a special hardware way of sharing keys using quantum physics, is not the same as post-quantum cryptography and is not a drop-in replacement for the bank's certificate estate.

## Quantum and the other topics

- Quantum computing is not blockchain, although both use cryptography language.
- Quantum computing is not generative AI.
- A quantum computer does not automatically make a faster core banking server.
- The immediate control is cryptographic planning, which belongs with the security chapter's TLS discussion.

**Say it in one line:** Qubits, superposition, and entanglement let quantum algorithms attack certain problems, including the mathematics behind RSA and elliptic curves. The present action for a bank is crypto-agility and a move toward post-quantum cryptography, not a quantum core-banking project.

---

# How the six fit in one bank

| Technology | One-line role | A safe first use |
| --- | --- | --- |
| Blockchain | Shared tamper-evident record among several parties | A permissioned trade-finance pilot with off-chain private data |
| IoT | Machines report their physical state | ATM cash and health monitoring |
| Cloud-native | Applications released and run as managed containers | A new channel service on an internal Kubernetes platform |
| Edge | Urgent or local processing near the branch or device | Branch alarm and buffering when the network blips |
| RPA | Bots do stable screen-level chores | Reconciliation with a human exception queue |
| Quantum | Future computing, present cryptographic risk | An inventory of public-key cryptography and a PQC migration plan |

## Mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "Blockchain is a Bitcoin account" | Blockchain is the ledger technology. A cryptocurrency uses a public blockchain. A bank use is more often a permissioned ledger for a process |
| "A smart contract is a legal contract" | It is code that executes rules. Legal rights must still be clear off the chain |
| "IoT is only consumer gadgets" | Any connected sensor or machine, including ATMs and data-centre equipment |
| "Cloud-native means it must run on a public cloud" | It is a building style. It can run in a private cloud or a modern data centre |
| "A container is a virtual machine" | A virtual machine includes a full operating system. A container packages the application and shares the host operating system, which makes it lighter |
| "Edge computing replaces the data centre" | Edge handles local urgency. The centre keeps the ledger and the enterprise view |
| "RPA is artificial intelligence" | RPA follows rules. It can be combined with AI. By itself it does not learn |
| "Quantum computers will speed up all bank programs soon" | They target specific problem classes. The near-term bank issue is future decryption of today's public-key cryptography |
| "AES is broken by Shor's algorithm in the same way as RSA" | Shor's algorithm threatens RSA and elliptic curves. AES needs adequate key length against a milder search speedup |

## Remember these lines

- Blockchain: blocks linked by hashes, agreed by consensus, hard to rewrite. Permissioned ledgers fit multi-party bank processes.
- IoT: sensors, connectivity, gateway, platform. Every device needs identity, patching, and network limits.
- Cloud-native: containers, Kubernetes, automation, observability, elastic copies.
- Edge: compute close to the event to save time, bandwidth, and dependence on a perfect network.
- RPA: a logged-in software identity that repeats stable rules and hands exceptions to people.
- Quantum: qubits and superposition. Future risk to RSA and ECC. Present response is post-quantum cryptography and crypto-agility.

# 7. Web Hosting and Cloud Deployment

## The one story

A web application is a set of programs and data. Those programs must run on a computer that is switched on, connected, secured, and looked after. **Hosting** is the provision of that home.

The home can be:

- a machine in the bank's own data centre, called **on-premises** or on-prem,
- a machine rented in someone else's data centre,
- or a pool of computing you draw from as a service, called the **cloud**.

**Deployment** means taking a built version of the application and putting it into an environment where it runs: development, test, or production.

**Say it in one line:** Hosting is where the application lives. Cloud is a way of renting that home as a flexible service. Deployment is the act of releasing a version into a chosen environment.

---

## The basic hosting pieces

Before cloud names, know the pieces every site needs.

| Piece | What it does |
| --- | --- |
| Server | The computer that runs the software |
| Operating system | Linux or Windows, the base software of that computer |
| Web server | Accepts HTTP and HTTPS |
| Application runtime | Runs the bank's program, such as Java on Tomcat |
| Database | Stores durable data |
| Domain name | The human name, such as `online.bank.example` |
| DNS | The phone book that turns that name into an IP address |
| IP address | The numeric address of a machine on the network |
| TLS certificate | Proves the site name and enables HTTPS |
| Firewall | Allows only the intended traffic |
| Data centre | A secure building with power, cooling, network, and physical access control |

### DNS in one slow pass

The customer types the site name. Their device asks a DNS resolver. DNS answers with an IP address. The device then connects to that address. If DNS is wrong or down, a perfectly healthy server can look "offline" to customers. DNS changes take time to spread. That delay is called **propagation**, and it is guided by a **TTL** on the DNS record.

Common DNS records:

| Record | Meaning |
| --- | --- |
| A | A name points to an IPv4 address |
| AAAA | A name points to an IPv6 address |
| CNAME | A name is an alias of another name |
| MX | Where email for the domain should go |

---

## Traditional hosting models

| Model | What you get | What you share | Fits |
| --- | --- | --- | --- |
| Shared hosting | A small site on a server used by many customers | The machine, and often the IP | A personal site. Not a bank channel |
| VPS, virtual private server | A virtual machine with its own operating system on shared hardware | The physical host, isolated by virtualisation | Small systems, trials |
| Dedicated server | A whole physical server for you | The data centre facilities | A workload that needs isolation and predictable hardware |
| Colocation | You own the server. The data-centre company provides space, power, cooling, and network | The building | A bank or a provider that wants to own the hardware and rent the hall |
| Managed hosting | The provider also patches and monitors | Depends on the contract | When the team wants operations included |
| On-premises | The bank's own data centre and staff | Little. The bank runs it | Core systems and data that policy says stay inside |

**Virtualisation** means one physical computer pretends to be several computers. Each virtual machine has its own operating system. This is older than cloud and is still how much of the cloud is built.

A bank channel normally lands on dedicated capacity, a private data centre, or a formally assessed cloud. Shared public web hosting is a study contrast, not a candidate for internet banking.

---

## Cloud, from the ground up

### The idea

A **cloud provider** runs huge data centres. You request computing, storage, and network when you need them, and you release them when you do not. You pay for what you use, under a contract. You do not buy a server for every peak and watch it sit idle in February.

Five traits often quoted, from the NIST description of cloud:

| Trait | Meaning |
| --- | --- |
| On-demand self-service | Authorised teams provision capacity through a portal or API, without a weeks-long hardware purchase for every small need |
| Broad network access | Services are reached over the network |
| Resource pooling | The provider's capacity is shared and allocated as customers need it. You usually do not pin a specific physical box |
| Rapid elasticity | Capacity can grow and shrink |
| Measured service | Usage is metered |

### Deployment models: who the cloud is for

| Model | Who uses it | Bank reading |
| --- | --- | --- |
| Public cloud | Many customers share the provider's cloud. Your data is logically separated | Useful for eligible workloads after risk assessment. Examples of providers: AWS, Microsoft Azure, Google Cloud |
| Private cloud | Used by one organisation. It may sit in the bank's data centre or be dedicated at a provider | Often preferred for sensitive banking workloads |
| Hybrid cloud | Some systems in the public cloud, some in the private cloud or on-prem, connected | A common real-world bank picture. The core may stay in the bank's data centre while a new digital journey uses a governed cloud |
| Community cloud | Shared by a group with a common mission or regulation | A sector cloud, where one exists |
| Multi-cloud | More than one public provider | Used to reduce reliance on a single provider, at the cost of extra skill and integration |

"Hybrid" is about where workloads live. "Multi-cloud" is about using more than one public provider. A bank can be both.

### Service models: how much you manage

This is the exam favourite. Remember it as a stack. The higher you go, the more the provider manages, and the less control you have over the internals.

| Model | You manage | The provider manages | Simple picture |
| --- | --- | --- | --- |
| IaaS, Infrastructure as a Service | Operating system, runtime, application, data | Physical machines, virtualisation, basic network and storage | You rent an empty flat. You bring your own furniture |
| PaaS, Platform as a Service | Application and data | The platform, runtime, scaling basics, operating system | You rent a furnished flat. You bring your belongings and live in it |
| SaaS, Software as a Service | Configuration and your use of it | The whole application | You book a hotel room. The hotel runs housekeeping |

Examples to make the names stick:

- IaaS: a virtual machine on AWS EC2, Azure Virtual Machines, or a similar service. You patch the operating system.
- PaaS: a service where you upload the application and the platform runs it, such as many "app service" offerings. You do not babysit the operating system.
- SaaS: web mail, a CRM such as Salesforce, or an HR portal you simply log in to.

Two more names sit beside these in current papers:

| Model | Meaning |
| --- | --- |
| FaaS, Function as a Service, also called serverless functions | You upload a small function. It runs when an event arrives. You do not manage servers. "Serverless" means you do not tend the server. A server still exists, run by the provider |
| CaaS, Containers as a Service | You give a container and the platform runs and scales it. Kubernetes services from cloud providers are in this neighbourhood |

**Serverless** does not mean "no computer". It means the capacity appears for the request and the provider owns the server operations. It suits spiky, event-driven tasks. A long, heavy, tightly controlled ledger process is usually a poor fit.

### The shared responsibility model

This is the sentence that keeps cloud reviews honest.

The provider is responsible for the security **of** the cloud: buildings, hardware, and the base platform.

The customer is responsible for security **in** the cloud: identity, data classification, configuration, application code, and who is allowed to open which storage.

A misconfigured storage bucket left open to the internet is the customer's failure even when the provider's data centre is excellent. Many public breaches are configuration failures of this kind.

As you move from IaaS to PaaS to SaaS, more responsibility shifts to the provider. It never shifts away completely. The bank still owns the data and the decision to use the service.

---

## Cloud words a manager should recognise

| Word | Simple meaning |
| --- | --- |
| Region | A geographic area of a cloud, made of data centres. Choosing a region affects latency and which law applies |
| Availability zone | A separate data centre, or group, inside a region. Spreading across zones survives the loss of one hall |
| VPC or virtual network | Your private network inside the cloud. You decide the subnets and the routes |
| Security group | A firewall around a cloud resource |
| Object storage | Stores files as objects, such as statement PDFs. AWS S3 and Azure Blob Storage are examples |
| Block storage | Disks attached to virtual machines |
| IAM | Identity and Access Management. Users, roles, and permissions for cloud resources |
| KMS | Key Management Service. The provider's controlled place for encryption keys, or a hook to keys you control |
| Encryption at rest | Data stored on disk is encrypted |
| Encryption in transit | Data moving on the network uses TLS |
| Tenant | One customer of a shared platform. Isolation between tenants is essential |
| Reserved capacity | A committed amount, often cheaper, for a steady workload |
| Spot or preemptible capacity | Cheap capacity the provider can take back. Fine for disposable batch work. Unsafe for a payment API |
| Egress | Data leaving the cloud. Often charged, and relevant to design |
| Vendor lock-in | Becoming unable to leave a provider without a painful rebuild |
| Exit plan | How you would get your data and workloads out. A serious procurement item |
| RPO | Recovery Point Objective. How much data you can afford to lose, measured in time. An RPO of 15 minutes means backups or replication must not be more than 15 minutes behind |
| RTO | Recovery Time Objective. How quickly the service must be back |
| Backup | A restorable copy. A backup you have never restored is a hope, not a control |
| Disaster recovery | The plan and the spare capacity to run from another site after a serious failure |
| BCP | Business Continuity Planning. How the business continues, including people and process, not only servers |

---

## Deployment

### Environments

| Environment | Who uses it | Data | Purpose |
| --- | --- | --- | --- |
| Development | Builders | Fake or masked | Create the change |
| Test or QA | Testers | Fake or masked | Check functions |
| UAT, user acceptance test | Business users | Masked, production-like | Confirm the journey meets the need |
| Staging or pre-production | Release team | Production-like | A dress rehearsal |
| Production | Real customers | Real | The live service |
| DR | Nobody, until needed | Replica | The standby |

A change moves forward through these. It does not start in production. Production access is limited.

**Data masking** means replacing real customer data in lower environments with realistic fake data. Copying a full production customer base into a test server is a serious data risk.

### How a release happens

1. Source code is stored in version control, such as Git. Every change has an author and a history.
2. A **build** turns source into a runnable package.
3. Automated tests run.
4. The package is deployed to test, then UAT, then production, with approvals the bank's policy requires.
5. After go-live, monitoring watches errors and speed.
6. If the release is bad, a **rollback** returns to the previous good version, or a fix-forward is made under control.

### Deployment styles

| Style | What customers experience |
| --- | --- |
| Recreate | Old version stops, new version starts. There is downtime |
| Rolling | Copies are updated one by one. Capacity dips and the service continues |
| Blue-green | Two full environments. Blue is live. Green gets the new version. Traffic switches to green when ready. Switching back is the rollback |
| Canary | A small share of customers goes to the new version first. If it is healthy, the share increases |
| Feature flag | The code is deployed and a switch turns the feature on later, for everyone or for a pilot group |

Blue-green and canary reduce the fear of a big-bang release. They need discipline and enough environments.

### CI/CD

- **CI**, continuous integration, means developers merge changes often and an automated build and test runs each time. Defects are found small.
- **CD**, continuous delivery, means every good build is ready to deploy, and deployment is automated up to a controlled approval.
- Continuous **deployment** means a passing build goes to production without a manual gate. Many banks deliberately stop at delivery plus an approval, because of change governance. Know the difference.

A **pipeline** is the automated path: build, test, scan, deploy.

### Containers, because deployment now uses them

A **container** packages an application with the libraries it needs, in a standard box. Docker is the well-known tool for building and running containers. A container starts quickly and runs the same way in test and production.

**Kubernetes** orchestrates many containers: it places them, restarts them, and scales them. It is the operations captain. Containers are explained again under cloud-native platforms. Here, the point is that deployment often means "roll out a new container image", not "copy files onto a hand-built server".

### Configuration and secrets

- Configuration is the part that changes between environments: database address, feature switches, log level.
- Secrets are passwords, keys, and tokens.
- Both should be injected at runtime from a protected store. They should not be baked into the image or committed in source code.

### Infrastructure as code

**Infrastructure as code** means the servers, networks, and permissions are described in files and created by a tool, not by clicking a console from memory. Terraform and cloud-native templates are examples. The benefit is repeatability and review. A change to infrastructure can be seen in a pull request, like a change to code.

---

## A bank's cloud decision, said as a manager

Technology choice is only half of hosting. The other half is governance.

Questions that belong in the note:

1. What data will live there? Public content, masked test data, or customer financial data?
2. Which law and which regulator expectation applies? For Indian banks, data localisation and outsourcing directions from the Reserve Bank of India matter. Customer payment data and certain storage expectations have specific rules. The safe exam instinct is: regulated data does not move to a cloud just because it is convenient. The location, the contract, the audit rights, and the residual risk must be in order.
3. Is the provider and the design assessed for security, resilience, concentration risk, and exit?
4. Who holds the encryption keys?
5. Are production and non-production separated?
6. Are logs, backups, and time synchronisation good enough for investigation and audit?
7. Is there a tested DR plan with a known RPO and RTO?
8. Will a cloud region outage, or a single-provider outage, take down a critical journey?

You are not expected to quote circular numbers from memory unless your desk uses them. You are expected to show that cloud is a risk decision, not only an infrastructure fashion.

### Edge and hosting

Some work is hosted close to the customer or the branch, called the edge. The main idea is in the emerging-technologies chapter. The link: not all computing has to sit in the central cloud. Central cloud, bank data centre, and edge can cooperate.

---

## A deployment story

The team releases a new "download interest certificate" journey.

1. Code is merged and the pipeline builds a container.
2. Security scans look for known vulnerable libraries.
3. The container deploys to test. Automated tests and a tester walk through it.
4. UAT uses masked customer-like data. Business users sign off.
5. Production is a private-cloud Kubernetes cluster behind the API gateway, in two availability zones.
6. A canary sends 5 percent of eligible customers to the new version.
7. Latency and errors stay normal. The flag opens for everyone.
8. The previous image is kept so rollback is a routing change, not a reconstruction.
9. PDFs are stored in encrypted object storage. The application checks the customer's identity before handing out a file link.
10. The next morning, operations confirms backups and looks at the dashboard.

That is hosting and cloud deployment as a practice, not as a logo.

---

## Hosting mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "Cloud means there are no servers" | The provider runs the servers. In serverless, you stop managing them |
| "SaaS means the bank has no responsibility" | The bank still owns data, access, and use |
| "IaaS gives you an application. PaaS gives you a virtual machine" | IaaS gives infrastructure such as virtual machines. PaaS gives a platform to run your application. SaaS gives the finished application |
| "Public cloud means everyone can read your data" | Customers share the platform. Their data must be isolated. A wrong configuration can break that isolation |
| "Hybrid cloud and multi-cloud are the same" | Hybrid mixes private or on-prem with public. Multi-cloud uses more than one public provider |
| "Deployment means buying a server" | Deployment means releasing a version into an environment |
| "A backup that has never been restored is proven" | Restore tests prove a backup |

## Remember these lines

- Hosting is the home of the application. DNS, certificates, servers, and data centres are the basic furniture.
- Shared, VPS, dedicated, colocation, and on-premises are the traditional choices.
- Public, private, hybrid, and community describe who the cloud is for.
- IaaS, PaaS, and SaaS describe how much of the stack you still manage.
- Shared responsibility: the provider secures the cloud, the bank secures its configuration, identity, application, and data.
- A release moves through dev, test, UAT, and production, with secrets outside the code and a way to roll back.
- RPO is how much data you can lose. RTO is how fast you must return.

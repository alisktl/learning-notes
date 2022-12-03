# Designing Data-Intensive Applications
by Martin Kleppmann

- [Chapter 1: Reliable, Scalable, and Maintainable Applications](#chapter-1-reliable-scalable-and-maintainable-applications)
  - [Reliability](#reliability)
  - [Scalability](#scalability)
  - [Maintainability](#maintainability)

---

## Chapter 1: Reliable, Scalable, and Maintainable Applications
* Many applications today are `data-intensive` and not `compute-intensive`, meaning bigger problems are usually the amount of data, the complexity of data, and the speed at which it is changing.
* A data-intensive application is typically built from building blocks that provide commonly needed functionality. Many applications need to:
  * Store data so that they, or another application, can find it again later (`databases`)
  * Remember the result of an expensive operation, to speed up reads (`caches`)
  * Allow users to search data by keyword or filter it in various ways (`search indexes`)
  * Send a message to another process, to be handled asynchronously (`stream processing`)
  * Periodically crunch a large amount of accumulated data (`batch processing`)

### Thinking About Data Systems
* In this book, we focus on three concerns that are important in most software systems:
  * `Reliability` \
    The systen should continue to work `correctly` even in the face of `adversity`.
  * `Scalability` \
    As the system *grows*, there should be reasonable ways of dealing with that growth.
  * `Maintainability` \
    Over time, many different people will work on the system, and they should all be able to work on it `productively`.

### Reliability
* For software, typical expectations include:
  * The application performs the function that the user expected.
  * Tolerate the user making mistakes.
  * Its performance is good enough for the required use case.
  * The system prevents any unauthorized access and abuse.
* `Reliability` means the system continues to work correctly, even when things go wrong.
* The things that can go wrong are called `faults`, and systems that anticipate faults and can cope with them are called `fault-tolerant` or `resilient`.
* Note that a fault is not the same as a failure. A `fault` is usually defined as one component of the system deviating from its spec, whereas `failure` is when the system as a whole stops providing the required service to the user.
* It is impossible to reduce the probability of a fault zero; therefore it is usually best to design fault-tolerance mechanisms that prevent faults from causing failures.

#### Hardware Faults
* Hard disks crash, RAM becomes faulty, the power grid has a blackout, someone unplugs the wrong network cable. Anyone who has worked with large datacenters can tell you that these things happen *all the time* when you have a lot of machines.
* It is unlikely that a large number of hardware components will fail at the same time.

#### Software Errors
* Software errors are systematic faults within the system.
* Such faults are harder to anticipate, and becaust they are correlated across nodes, they tend to cause many more system failures than uncorrelated hardware faults.

#### Human Errors
* Humans are known to be unreliable. For example, one study of large internet services found that configuration errors by operators were the leading cause of outages, whereas hardware faults played a role in only 10-25% of outages.
* We can make systems more reliable:
  * Design systems in a way that minimizes opportunites for error. For example, well-designed abstractions, APIs, and admin interfaces make it easy to do "the right thing" and discourage "the wrong thing."
  * Decouple the places where people make the most mistakes from the places where they can cause failures. In particular, provide fully features non-production `sandbox` environments where people can explore and experiment safely, using real data, without affecting real users.
  * Test thoroughly at all levels, from unit tests to while-system integration tests and manual tests.
  * Allow quick and easy recovery from human errors, to minimize the impact in the case of failure.
  * For example:
    * Make it fast to roll back configuration changes.
    * Roll out new code gradually.
    * Provide tools to recompute data (in case it turns out that the old computation was incorrect).
  * Set up detailed and clear monitoring, such as performance metrics and error rates.
  * Implement good management practices and training.
 
### Scalability
* `Scalability` is the term we use to describe a system's ability to cope with increased load.

#### Describing Load
* Load can be described with a few numbers which we call `load parameters`.
* The best choice of parameters depends on the architecture of your system: it may be
  * requests per second to a web server
  * the ratio of reads to writes in a database
  * the number of simultaneously active users in a chat room
  * the hit rate on a cache

#### Describing Performance
* You can look at what happens when load increases in two ways:
  * When you increase a load parameter and keep system resources fixed, how is the performance of your system affected?
  * When you increase a load parameter, how much do you need to increase the resources if you want to keep performance unchanged?
* `Response time` what the client sees, including the time to process the request (the service time) and network delays and queuing delays.
* `Latency` is the duration that a request is waiting to be handled - during which it is *latent*, awaiting service.
* The `arithmetic mean` is not a good metrics to describe the *typical* response time, as it doesn't tell you how many users experienced that delay. Prefer `percentiles` instead.
* *High percentiles* of response times, or *tail latencies*, are important because they directly affect users' experience of the service.
* Averaging percentiles, such as when combining data from several machines, is mathematically meaningless. The right way of aggregating response time data is to add the histograms.

#### Approaches for Coping with Load
* `Scaling up`, or `vertical scaling`, is moving to a more powerful machine. `Scaling out`, or `horizontally scaling`, distributes the load across multiple smaller machines.
* An architecture that scales well for a particular application is built around assumptions of operations will be common and which will be rare - the load parameters.
* In an early-stage startup it's more important to be able to iterate quickly on product features than to scale beyond some hypothetical future load.

### Maintainability
* The majority of the cost of software is not in its initial development, but its ongoing maintenance.
* Three important design principles for maintainability are:
  * `Operability`: make it easy for the operations teams to keep the system running smoothly.
  * `Simplicity`: make it easy for new engineers to understand the system by removing complexity.
  * `Evolvability` (or `extensibility`): make it easy for engineers to adapt the system to unanticipated use cases as requirements change.

#### Operability: Making Life Easy for Operations
* A good operations team is responsible for:
  * Monitoring and quickly restoring if it goes into bad state.
  * Tracking down the cause of problems.
  * Keeping software and platforms up to date.
  * Keeping tabs on how different systems affect each other.
  * Anticipating future problems.
  * Establishing good practices and tools for development.
  * Perform complex maintainance tasks, like platform migration.
  * Maintaining the security of the system.
  * Defining processes that make operations predictable.
  * Preserving the organization's knowledge about the system.
* Good operability means making routine tasks easy.

#### Simplicity: Managing Complexity
* Making a system simpler does not necessarily mean reducing its functionality; it can also removing *accidental complexity*.
* `Accidental complexity` is complexity that is not inherent in the problem that the software solves, but arises only from the implementation.
* Good abstractions are one of the best tools for removing accidental complexity by hiding implementation details, but finding good abstractions is very hard.

#### Evolvability: Making Change Easy
* Simple and easy-to-understand systems are usually easier to modify than complex ones.

---

## Chapter 2: Data Models and Query Languages

# Designing Data-Intensive Applications
by Martin Kleppmann

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

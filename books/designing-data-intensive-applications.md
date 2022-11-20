# Designing Data-Intensive Applications
by Martin Kleppmann

## Chapter 1: Reliable, Scalable, and Maintainable Applications
* Many applications today are *data-intensive* and not *compute-intensive*, meaning bigger problems are usually the amount of data, the complexity of data, and the speed at which it is changing.
* A data-intensive application is typically built from building blocks that provide commonly needed functionality. Many applications need to:
  * Store data so that they, or another application, can find it again later (*databases*)
  * Remember the result of an expensive operation, to speed up reads (caches)
  * Allow users to search data by keyword or filter it in various ways (*search indexes*)
  * Send a message to another process, to be handled asynchronously (*stream processing*)
  * Periodically crunch a large amount of accumulated data (*batch processing*)

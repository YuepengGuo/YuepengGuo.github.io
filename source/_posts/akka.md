
## Responsive

 * Resilient: Applications that can heal themselves, which means they can recover from failure, and will always be responsive, even in case of failure like if we get errors or exceptions

 * Elastic: A system which is responsive under varying amount of workload, that is, the system always remains responsive, irrespective of increasing or decreasing traffic, by increasing or decreasing the resources allocated to service this workload

 * Message Driven: A system whose components are loosely coupled with each other and communicate using asynchronous message passing, and which reacts to those messages by taking an action


 ## Fault Tolerance

  * Breaking the system into components: While designing a fault-tolerant system, the first requirement is to break the system into parts, that is, components which are each responsible for some functionality. Certain failure in one component of the system should not interfere with the other parts of the system, and should not bring cascading failures in the system.

  * Focus on the important components of the system: There are some parts that are important for a system to have. Such parts should run without interference from the failing parts to avoid inaccurate results.
  
  * Backup of important components: It is recommended to have a backup of components so that in case of failure, similar components can ensure the high availability of the system.
  
  * Duplication: The purpose of duplication is to run multiple identical instances of system components so that if a failure occurs, other instances will be available to process the request

  * Replication: The purpose of replication is to provide multiple identical instances of the components (hardware and software) and to send a direct request to all of them one of the results is then chosen from among them based on them

  * Isolation: The purpose of isolation is to keep the components running in different processes, and communicate through passing of messages to ensure isolation of concerns and loose coupling between them. This means that one should not be affected because of another's failure

  * Delegation: The purpose of delegation is to hand over the processing responsibility of a task to another component so that the delegating component can perform other processing, or optionally observe the progress of the delegated task in case additional action, such as handling failure or reporting progress, is required

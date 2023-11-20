- 4 priorities
  id:: 655bb258-b506-47d8-8a3d-812ffc646fbe
	- Defined in enum `ProcessPriority`
	- `SYSTEM = 0`
	  id:: 655bb35a-b8e1-49e8-bd05-047e2fc1f59a
	- `INTERACTIVE = 1`
	- `NORMAL = 2`
	- `BATCH = 3`
- `Stable_Prioriry_Queue`
  id:: 655bb39e-e38e-4ff0-ac20-bfc357b8f8a7
	- Data structure provided for circumstances that may benefit from using a priority queue.
	- Use in place to `std::priority_queue`
	- Top of queue will be the earliest added element in the queue that has the lowest priority value.
- # Algorithms
	- ## Priority
	  id:: 655bb1a3-335e-4382-a5d9-050fbc627947
		- ### Overview
			- There are ((655bb258-b506-47d8-8a3d-812ffc646fbe))
			- We want to run the oldest process with the lowest priority (in value, lower values = higher priority) in the queue. This means all threads with a priority of `SYSTEM` should run before any thread with `INTERACTIVE`.
		- ### Implementation
			- Use a ((655bb39e-e38e-4ff0-ac20-bfc357b8f8a7)) for underlying data structure.
	- ## Multi-level Feedback Queue
		- ### Overview
			- Basically multiple ((655bb1a3-335e-4382-a5d9-050fbc627947)) implementations.
		- ### Implementation
			- Use an array or vector of 10 ((655bb39e-e38e-4ff0-ac20-bfc357b8f8a7))s.
				- Every queue level has a different time slice, equal to $2^i$, where $i$ is the queue index.
				  collapsed:: true
					- The 0th queue will have a time slice of 1.
					- The 1st queue will have a time slice of 2.
					- The 2nd queue will have a time slice of 4.
					- And so on...
			- Threads need to store additional information:
				- Current queue level.
				- Sum of service time spent in current queue level.
			- When selecting a thread:
				- Loop through queue level (0-9) until you find a non-empty queue.
				- Pop off and use the thread at the top of that queue.
				- Set the time slice according the queue the thread came from.
			- When inserting a thread:
				- Check if the service time the thread has spent in its queue has exceeded that queues time slice.
				- If the time slice has been exceeded, and the thread is not already in the bottom queue:
					- Increment the queue index (thus setting the thread to a lower queue).
					- Reset the time the thread has spent in its present queue (since it is now going in a new queue).
					- Insert into the new queue.
				- If the time slice has not been exceeded, or the thread is already in the bottom queue:
					- Reinsert into the thread's queue level.
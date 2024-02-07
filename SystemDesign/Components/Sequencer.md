# Sequencer
With distributed systems, we end up with a lot of content all over the place, and we need a way to quickly identify content. This is when a sequencer comes in, which can generate a unique identifier to be associated with content.
This can look like a generated UUID to be associated with an event to track a user session and collected data. 
We can split the design of a sequencer into two parts:
1. **Unique ID Generator**: we first need a way to actually make the IDs and our creation mechanism needs to ensure that the IDs are actually unique, as any collisions would be bad.
2. **Unique ID with Causality**: if the content needs to be chronologically organized (timestamped) we will also need a way to track when these unique IDs have been generated.
## Things to Keep in Mind
- Solves the issue of giving content a unique ID so it's easy to get content just from an ID
- Duplicate IDs are very very bad
- Bigger keys means slower update time and more overhead with creating and updating IDs
- Sometimes all you need is a simple integer counter to keep track of event data
- For security purposes, it can be beneficial to make it difficult to predict what the next ID is, it can also cause "hotspots" in ID generation, leading to unbalanced content
- Fetching a timestamp is slower than incrementing a counter
- Globally ordering everything is expensive, if you can avoid doing that then I would
## Unique ID Generator Design
### Functional Requirements
- **Uniqueness**: generated IDs must be unique for this to work
- **64-bit numeric ID**: the textbook for the system design wanted this, not technically necessary 
### Non-Functional Requirements
- **Scalability**: The ID generation system should generate at least a billion unique IDs per day
- **Availability**: Events can happen at the nanosecond level, and we need to capture these events
### Potential Solution: [[Universally Unique Identifiers|UUIDs]]
**Universally Unique Identifiers** (UUIDs) solves most of the requirements
- Each UUID generated is unique
- No coordination is required for UUIDs to ensure no collisions between them
- Not 64-bit, UUID is 128-bit and non-numeric, so if that's a deal breaker then so be it
### Potential Solution: [[Databases|Database]]
We use a database, where we have full control over the ID generation but it struggles with some other requirements.
- Cannot ensure uniqueness with the given values if a node fails when a event ID is being generated 
- Adding and removing servers can be tough to do when you need to coordinate servers so there are no duplicate IDs
### Potential Solution: [[Range Handler]]
UUIDs and Databases cover a lot of the requirements, but not all of them. In this case a Range Handler makes more sense as a solution.
Servers can claim a range of IDs, and then if they run out of IDs or are making IDs for the first time, they can claim another range.
- Coordinating and assigning ranges solves the issue of creating duplicate IDs
## Unique ID with Causality Design
Having unique IDs is great, but sometimes that's not enough. In the context of Social Media, we also need to know when posts are made, so content can be properly sorted chronologically.
Some events don't need to be sorted by time, known as **concurrent events** which can happen at the same time with no side effects, e.g. two people writing Tweets at the same time has no impact on the other.
On the other hand, some events do need to be sorted by time to ensure validity, known as **non-concurrent events**, e.g. someone makes a post, and then someone comments on that post. A user cannot comment on a post before it's made, and thus we cannot perform both events simultaneously.
### Potential Solution: [[UNIX Timestamps]]
- Easy way to add a timestamp to an event
- Leads to problem when events happen in the same nanosecond
### Potential Solution: [[Logical Clocks]]
Logical clocks are like timestamps but only increment when an event occurs, depending on the type of logical clock used, causality may or may not be able to be determined at the global level.

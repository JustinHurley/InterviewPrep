# Fault Tolerance
A system's ability to execute persistently even if one or more of it's components fail (software or hardware).

## Fault Tolerance Techniques
#### Replication
- Make multiple copies of nodes, so if one fails we can just switch over to the other copy 
#### Checkpointing 
- Basically just backing up the data in-case the system ends up in an irrecoverable state
- Types of Checkpointing 
	- **Consistent State**: All checkpoints across all nodes are consistent and the same
	- **Inconsistent State**: There may be discrepancies across different checkpoints 
	- Main difference between the two is that in an inconsistent state, node A could take a snapshot, send a message to node B, which would take in the snapshot and then there would be a disparity in state between node A and node B
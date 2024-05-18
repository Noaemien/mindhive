---
Week: 10
Themes: 
aliases: 
Lecture1: true
Lecture2: false
Exercises: false
---

# Lecture 1

## Reliable data delivery
- avoid packet corruption

Transport layer can check for data corruption
### Checksum
- Redundant information
	- e.g., the binary sum of all data bytes
- Sender adds checksum C to each segment
	- transport-layer header field
- Receiver uses it to detect data corruption
	- receiver recomputes checksum C'
	- if C' != C, segment was corrupted

### Acknowledgement
- Feedback from receiver to sender
- Receiver adds ACK to each segment
	- transport-layer header field
- Sender uses it to detect and overcome data corruption
	- if sender gets negative ACK, it re-transmits the data

### Sequence numbers
- Identifier for data
- Sender adds SEQ to each segment
	- transport-layer header field
- Receiver uses it to disambiguate data

### Timeouts
- No arrival of an expected ACK
	- a segment was lost or delayed
	- the ACK for a segment was lost or delayed

### Go-back-N
- The receiver accepts no out-of-order segments
- ACKs are cumulative
	- an ACK for segment 10 indicates that all segments until and including 10 have been recieved
- When sender retransmits,
- it retransmits all the un-ACK-ed segments

### Selective repeat
- The receiver accepts N-1 out-of-order segments
- ACKs are selective
	- an ACK for segment 10 indicates that segment 10 has been received
- When the sender re-transmits, it re-transmits only one segment 

## Speed

### Stop and wait
- the sender does nothing while waiting for the receiver's ACK or the timeout
- poor sender utilization
### Pipelining
- the sender sends up to N un-ACKed segments
- N = sliding window size
- better utilization
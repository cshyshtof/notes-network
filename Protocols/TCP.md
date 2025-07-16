---
type: zettel
---

```meta-bind-button
label: Set next review
hidden: false
id: review
style: primary
actions:
  - type: command
    command: review-obsidian:future-review
  - type: input
    str: "next 7 days"
```

# TCP

- Header
	- Offset
		- TCP header length
		- The same rules apply as for IP header
	- SN
		- Initial SNs for new sessions increments every 0.5 sec
		- The increment is by 64000, cycling to 0 after about 9,5h
		- Each connection starts with different initial numer
	- Flags
		- CWR
			- Congestion Window Reduced
			- Set by the sending host
			- Indicates that a segment was received with the ECE flag set
			- Host indicates it had responded with congestion control mechanism
		- ECE
			- Explicit Congestion Notification (ECN-Echo)
			- Not the same as ECN in IP header TOS field
		- URG
			- Indicates that the Urgent pointer field is to be inspected
		- ACK
			- Acknowledges data received
			- All packets after the initial SYN should have this flag set
		- PSH
			- Asks to immediately push the buffered data to the application
			- Normally, TCP waits for the buffer to exceed the MSS
			- Buffering can introduce a delay for applications sending small data
		- RST
			- Reset the connection
		- SYN
			- Only the first packet sent from each end should have this flag set
		- FIN
			- No more data will be sent from a sender, connection can be closed
	- Options
		- MSS, Timestamp, Selective ACK
		- Exchanged only in first segments (SYN)
		- `(G) ip tcp selective-ack`; - Multiple packets can be lost from one window of data; - A receiver returns selective ACKs to a sender; - Informs about data that has been received; - The sender can then resend only the missing data segments
		- `(G) ip tcp timestamp`; Improves round-trip time estimates
	- Urgent Pointer
	- Window size
		- `(G) ip tcp window-size <bytes>`
		- Defines the number of bytes a receiver is willing to accept before ACK
		- Initially set to a number of bytes set as ACK SN sent in 3-way handshake
		- Default is 4128 B


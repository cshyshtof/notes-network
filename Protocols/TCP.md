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
- Handshake
	- 3-way handshake is required before data can be sent
	- Each side sets own SN independently, and exchanges it with the other side
	- Closing connection is a 4-way
	- Any endpoint can send FIN to signal EoT, it must be ACKed
	- Since TCP is a full-duplex, other side must also send FIN and wait for ACK
- MSS
	- `(G) ip tcp mss <#>`; MSS for TCP connections initiated from a router; Default is 1460 for local (no IP/TCP headers) or 536 for remote destinations
	- MSS is a largest amount of data (without headers) that TCP is willing to send
	- MSS = MTU – IP/TCP headers
	- Should be small enough to avoid fragmentation
	- A sender selects the lower of own MSS or local MTU and sends as MSS
	- If destination IP is non-local or other side does not set MSS, MSS is set to 536
	- Received MSS is always compared only to local MTU – smaller value is used
	- If there is a smaller MTU somewhere on the path, fragmentation will occur
- Flow
	- A receiver sets the receive window with the amount of data it can buffer
	- A sender can send only up to that amount of data before it waits for ACK
	- Windows is set in every segment, and is floating
	- Windows depend on how fast a process reads data from incoming buffer
	- ACK with a window 0 means a receiver hasn’t read data from buffer yet
	- Next ACK can be sent as a window update, even if no new data was received
	- TCP usually does not send ACK at the same time data has been received
	- It waits 200ms so maybe some data can be send back (piggyback ACK)
	- If there is data to be sent, ACK is send immediately
	- The timer goes off at fixed intervals, so ACK can wait from 1 to 200 msec
	- Naggle
		- Collect data and send them in one segment to avoid tinygrams
		- Not recommended for interactive applications (mouse movements)
- Timeout
	- The time difference between sent packet and received ACK is the RTT
	- For calculating the network delay variation, RTT is used to calculate SRTT
	- The sender uses SRTT to calculate retransmission timeout (RTO)
	- The sender continuously adjusts the RTO based on the SRTT and RTT
	- These timers are relevant only for TCP
	- If ACK is not received before RTO expires, the sender retransmits that packet
	- This is called timeout-based retransmission
- BW-Delay Product
	- Data link's capacity in bps and its RTT in sec.
	- Maximum amount of data on the network circuit at any given time
	- Data that has been transmitted but not yet acknowledged
	- LFN
		- Long Fat Network
		- A pipe with high bandwidth, but also large delay
		- The solutions for LFN is to have bigger TCP window
- Congestion
	- 3-way sets CWND to 1 and Slow Start Threshold (SSTHRESH) to 65535
	- CWND varies much quicker than Advertised Window, as it reacts to congestion
	- A sender always uses the lower of the two windows
	- Process
		- A sender fails to receive ACK in time (possible lost packet)
		- A sender sets CWND to the size of a single segment
		- SSTHRESH is set to 50% of CWND value before a segment was lost
		- Slow start governs how fast CWND grows until it reaches SSTHRESH
		- Then, congestion avoidance governs how fast CWND grows
	- CWND grows at an exponential rate during slow start
	- Congestion avoidance allows CWND to grow slower at a linear rate
	- Congestion avoidance and Slow Start are algorithms with different objectives
	- In practice they are implemented together


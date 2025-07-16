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

# UDP

- Concepts
	- Connectionless, no way to track lost datagrams
	- Upper layer must take care of retransmissions
	- Host is not required to receive datagram larger than 576 bytes
	- UDP protocols often limit their payload to 512 bytes
	- Checksum
		- Calculated from IP and UDP headers and data padded with zero
		- Padding is to a multiple of two octets (IP pseudo-header)

---

![[udp-header.png]]
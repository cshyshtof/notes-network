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

# IPv4

- Header
	- Header Len
		- Number of 32b/4B words – default is 5, that is 5x4 bytes = 20 bytes
		- Max IP header is 60 bytes (15x4B words)
		- Padding is used to make sure header always end on 32 bits boundary
	- Total length
		- Entire datagram size, including header, in 32 bit words. Max 65536B
	- Identification
		- Identifies fragments of an original IP datagram
	- Flags
		- Bit 0: Reserved
		- Bit 1: Don't Fragment (DF)
		- Bit 2: More Fragments (MF)
	- Fragment offset
		- Defined in 8B blocks
		- Specifies the offset, relative to the beginning of the original datagram
		- The first fragment has an offset of zero
		- Maximum offset is (2^13 – 1) × 8 = 65,528 bytes
	- TTL
		- Each L3 device decrements TTL by one
		- When it hits zero, the packet is discarded
	- Header checksum
		- Calculated and verified at each L3 hop
	- IP options
		- Record route, timestamp, loose/strict source routing, enhanced traceroute
		- Type
			- Coppied 1b (copy option information to all fragments)
			- Class 2b (0:control, 2:debugging)
			- Number 5b (what kind of option)
		- Length
			- 8b – total length of the option
		- `(G) ip options {drop | ignore}`


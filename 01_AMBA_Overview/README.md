# AMBA Overview

### AXI channels
* The AXI specification describes a point-to-point protocol between two interfaces: a manager and a subordinate. The following diagram shows the five main channels that each AXI interface uses for communication:

<img src="https://github.com/user-attachments/assets/b7324871-157e-4c3b-abb4-bb84c03618f7" width=600>

---
---
Each of these five channels contains several signals, and all these signals in each channel have the prefix as follows:

* AW for signals on the Write Address channel
* AR for signals on the Read Address channel
* W for signals on the Write Data channel
* R for signals on the Read Data channel
* B for signals on the Write Response channel
  
---
### Write operations use the following channels:

The manager sends an address on the Write Address (AW) channel and transfers data on the Write Data (W) channel to the subordinate.
The subordinate writes the received data to the specified address. Once the subordinate has completed the write operation, it responds with a message to the manager on the Write Response (B) channel.

Using separate address and data channels for read and write transfers helps to maximize the bandwidth of the interface. There is no timing relationship between the groups of read and write channels. This means that a read sequence can happen at the same time as a write sequence.

---
### Read operations use the following channels:

The manager sends the address it wants to read on the Read Address (AR) channel.
The subordinate sends the data from the requested address to the manager on the Read Data (R) channel. The subordinate can also return an error message on the Read Data (R) channel. An error occurs if, for example, the address is not valid, or the data is corrupted, or the access does not have the right security permission.

---
* Reference
  * https://developer.arm.com/documentation/102202/0300/AXI-protocol-overview

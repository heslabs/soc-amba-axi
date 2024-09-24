# AMBA Overview

This guide introduces the main features of Advanced Microcontroller Bus Architecture (AMBA) AXI. The guide explains the key concepts and details that help you implement the AXI protocol.

## Key AMBA specifications
AMBA has evolved over the years to meet the demands of processors and new technologies, as shown in the following diagram:
  
<img src="https://github.com/user-attachments/assets/beb16846-a03e-4b83-98c4-ea141201d521" width=900>


---
### System diagram of an SoC with multiple AXI interfaces
* There are only two AXI interface types, manager and subordinate. These interface types are symmetrical.
* All AXI connections are between manager interfaces and subordinate interfaces.
* AXI interconnect interfaces contain the same signals, which makes integration of different IP relatively simple.
  
<img src="https://github.com/user-attachments/assets/ba59cdfa-7dd1-4cf7-a4df-6851de14c91a" width=700>

---
### System diagram of an SoC with multiple AXI/CHI interfaces

  
<img src="https://github.com/user-attachments/assets/6ac32bc2-760f-4fed-9a89-0c31d3bac235" width=700>

---
## AXI channels
* The AXI specification describes a point-to-point protocol between two interfaces: a manager and a subordinate. The following diagram shows the five main channels that each AXI interface uses for communication:

<img src="https://github.com/user-attachments/assets/b7324871-157e-4c3b-abb4-bb84c03618f7" width=600>

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
### Main AXI features
The AXI protocol has several key features that are designed to improve bandwidth and latency of data transfers and transactions:

* **Independent read and write channels**
  * AXI supports two different sets of channels, one for write operations, and one for read operations. Having two independent sets of channel helps to improve the bandwidth performances of the interfaces. This is because read and write operations can happen at the same time.
* **Multiple outstanding addresses**
  * AXI allows for multiple outstanding addresses. This means that a manager can issue transactions without waiting for earlier transactions to complete. This can improve system performance because it enables parallel processing of transactions.
* **No strict timing relationship between address and data operations**
  * With AXI, there is no strict timing relationship between the address and data operations. This means that, for example, a manager could issue a write address on the Write Address channel, but there is no time requirement for when the manager has to provide the corresponding data to write on the Write Data channel.
* **Support for unaligned data transfers**
  * For any burst that is made up of data transfers wider than one byte, the first bytes accessed can be unaligned with the natural address boundary. For example, a 32-bit data packet that starts at a byte address of 0x1002 is not aligned to the natural 32-bit address boundary.
* **Out-of-order transaction completion**
  * Out-of-order transaction completion is possible with AXI. The AXI protocol includes transaction identifiers, and there is no restriction on the completion of transactions with different ID values. This means that a single physical port can support out-of-order transactions by acting as several logical ports, each of which handles its transactions in order.
* **Burst transactions based on start address**
  * AXI managers only issue the starting address for the first transfer. For any following transfers, the subordinate will calculate the next transfer address based on the burst type.

---
## Channel transfers and transactions

#### Channel handshake
The AXI4 protocol defines five different channels, as described in AXI channels. All of these channels share the same handshake mechanism that is based on the VALID and READY signals, as shown in the following diagram:

<img src="https://github.com/user-attachments/assets/a034e9b6-582f-49e9-b54c-0a6e77459176" width=600>

---
#### Differences between transfers and transactions
* When designing interconnect fabric, you must know the capabilities of the managers and subordinates that are being connected. Knowing this information lets you include sufficient buffering, tracking, and decode logic to support the various data transfer ordering possibilities that allow performance improvements in faster devices.
* Using standard terminology makes understanding the interactions between connected components easier. AXI makes a distinction between transfers and transactions:
  * A transfer is a single exchange of information, with one VALID and READY handshake.
  * A transaction is an entire burst of transfers, containing an address transfer, one or more data transfers, and, for write sequences, a response transfer.


---
### Write transaction: single data item
* The process of a write transaction for a single data item, and the different channels that are used to complete the transaction.
* This write transaction involves the following channels:
  * Write Address (AW)
  * Write (W)
  * Write Response (B)

<img src="https://github.com/user-attachments/assets/7dc65f8e-5c9d-4cda-a286-4f528d98d350" width=800>

---
### Read transaction: single data item
* The process of a read transaction for a single data item, and the different channels used to complete the transaction.
* This write transaction involves the following channels:
  * Read Address (AR)
  * Read (R)

<img src="https://github.com/user-attachments/assets/4f5ccc1d-f83c-4e35-b36b-c84430b9dec6" width=800>


---
## Examples of simple transactions
Examples of simple transactions help to explain the relationships between the different AXI channels.
The following diagram shows a time representation of several valid transactions on the five channels of an AXI3 or AXI4 interface:

### A time representation of several valid transactions
<img src="https://github.com/user-attachments/assets/3bc1f806-1379-497e-90cd-26b08f0d9749" width=800>

### Same sequence of read and write transactions in a different timeline
<img src="https://github.com/user-attachments/assets/2d8f5510-0144-4e0b-8478-d9216896329d" width=800>


---
* Reference
  * https://developer.arm.com/documentation/102202/0300/AXI-protocol-overview

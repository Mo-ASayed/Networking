# Layer 4 - Transport Layer

The transport layer runs `ontop of the Network layer` and provides most of the functionality that supports most of the networking that is used day-to-day.

## Problems with Layer 3

A Source IP can generate multiple different packets destined for the `same destination IP`, however each packet is routed independently from the other.
A fair assumption is that the packets arrive at the same time, same order and same quality.. however this is NOT true.

This is the issue with communicating using only ip, causing intermittent network conditions, causing cases where arrival conditions are different to when they were generated.

One issue that can occur is `Out of Order Arrival` where packet 2 arrives AFTER 3. Routing decisions are `per packet`. Different routes can result in out of ordering packets at the destination and layer 3 provides NO ordering mechanism.

Packets can also go `Missing` due to network outages or conditions which cause temporary routing loops. So Layer 3 communication is `not guaranteed`.

Per packet routing can also have `delays`. Different packets can experience different delays which can be a serious issue for `time sensitive applications`.

### Key point to remember about layer 3: 
`Each Packet is Unique, Different, Isolated. What happens to one packet doesn't mean it will happen to the other packets.`

With layer 3, there are `no communication channels` which means they have a Source and Destination IP but not a method of splitting by APP or Channel.

IP has `no flow control`. If a source transmits faster than the destination can recieve it, it can saturate the destination causing packet loss.


## Layer 4

Layer 4 adds 2 new protocols.

TCP - Transmission Control Protocol
UDP - User Datagram Protocol

Both run ontop of IP. There are common terms that you might hear like `TCP/IP`, this is TCP is running on top of IP, you choose this for `Reliablity, error correction and ordering of data` and is used for most important application layer protocols (Layer 7) like HTTP, HTTPS and SSH.

TCP is a `connection oriented protocol` - this means you need a connection between 2 devices setting up a bi-directional channel for communication.

UDP - Is faster because it doesnt have the TCP overhead for the reliable transfer of data, simply means its less reliable.

TCP is more `RELIABLE` but UDP has better `PERFORMANCE`.

![alt text](./OSI_Images/TCP_UDP.png)

## TCP Segments

A Segment is simply just another `Container` for data like packets and frames in previous layers and are specific to TCP.

Key Note - Segments are contained in IP packets!

TCP Segments `are encapsulated within IP Packets` and are carried from Source to Destination. However, Segments `do NOT have Source or Destination IPs` as they use the IP packets (handled by Layer 3).

### Segment Structure

Source Port and Destination Port - Gives the combined TCP/IP protocol the ability to have `multiple streams` of converstions between 2 devices.

For example, Opening the AWS Web Interface we are communicating from a port on our local machine to a port on the AWS server, which is `TCP Port 443 - HTTPS`

`Source + Destination IP` + `Source and Destination Port`these together identify a `single converstation`. This concept is what allows SSH and HTTPs to exist on a `Single EC2 Instance` for example. 

Sequence Number - Is incrememnted with each segment that is sent and is `unique`. Can be used for error correcting and for when IP Packets are recieved and the TCP segments are pulled out, they can be correctly ordered - solving an issue from Layer 3.

Acknowledgment - Is the way one side can indicate that its recieved uppto and including a certain sequence number. This is because every segment that is sent *NEEDS* to be `acknowledged` because TCP is a `reliable protocol`.

Flags and other fields (*) - 9 Bits and has various controls over the TCP connection. Are used to close the connection or syncrhonise the sequence numbers. 

TCP Window - Defines the number of `bytes` that you indicate you're willing to recieve between acknowledgments. Once reached, the sender will pause until you acknowledge that amount of data. This is how `Flow Control` is achieved. 

Larger windows are more efficient due to the `Header` of a TCP segment taking up more space, and the smaller the window between allowing data to be recieved, the `more headers are involved`.

Checksum - Used for error checking, can arrange for re-transmission of data as required.

Urgent Pointer - This is a `16 bit` field that indicates the `location` of the last byte of urgent data in a TCP Segment. Simply put the Urgent data is data that needs to be processed ASAP.

From Source and Destination Port to Options and Paddings (Which aren't currently mentioned but sit below Checksum and Urgent Point) - These all together make up the `TCP Header`.
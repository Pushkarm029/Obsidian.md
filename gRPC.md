
### RPC Types :

1. Unary RPCs where the client sends a single request to the server and gets a single response back, just like a normal function call.
    ```proto
	rpc SayHello(HelloRequest) returns (HelloResponse);
    ```

2. Server streaming RPCs where the client sends a request to the server and gets a stream to read a sequence of messages back. The client reads from the returned stream until there are no more messages. gRPC guarantees message ordering within an individual RPC call.
    ```proto
    rpc LotsOfReplies(HelloRequest) returns (stream HelloResponse);
	```

3. Client streaming RPCs where the client writes a sequence of messages and sends them to the server, again using a provided stream. Once the client has finished writing the messages, it waits for the server to read them and return its response. Again gRPC guarantees message ordering within an individual RPC call.
    ```proto
    rpc LotsOfGreetings(stream HelloRequest) returns (HelloResponse);
    ```

4. Bidirectional streaming RPCs where both sides send a sequence of messages using a read-write stream. The two streams operate independently, so clients and servers can read and write in whatever order they like: for example, the server could wait to receive all the client messages before writing its responses, or it could alternately read a message then write a message, or some other combination of reads and writes. The order of messages in each stream is preserved.
    ```proto
    rpc BidiHello(stream HelloRequest) returns (stream HelloResponse);
    ```

### Life of an RPC :
![[Pasted image 20231127141726.png]]

#### First Step : Create a Channel to transmit the RPC
![[Pasted image 20231127141827.png]]

![[Pasted image 20231127141914.png]]
![[Pasted image 20231127142030.png]]

#### Second Step : Create an RPC over the channel

##### Client Side :
![[Pasted image 20231127143657.png]]

##### Server Side :
![[Pasted image 20231127143825.png]]

##### Data Flow :
![[Pasted image 20231127143915.png]]



### Features :
- Pluggable components :
	- Resolver, balancer, IDL, compressor, codec, transport

- Rich Features :
	- Binary Logging
	- Channelz - is a tool that provides comprehensive runtime info about connections at different levels in gRPC. It is designed to help debug live programs, which may be suffering from network, performance, configuration issues, etc.
	- Health Checking
	- RPC retry
	- Tracing
	- Service Config


### Service Config
![[Pasted image 20231127150458.png]]
# DEZSYS_GK_HELLOWORLD_GRPC

## Short description

This is a small HelloWorld example with gRPC.

There is one server and one client.
The client sends a first name and last name to the server.
The server answers with:

```text
Hello World, Thomas Micheler
```

## What was done

- proto file created
- Java server created
- Java client created
- Gradle tasks for starting it
- server runs on port 50051

No DataWarehouse was made, because this is not part of the GK task.
A second client in another programming language was also not made.

## Used

- Java
- Gradle
- gRPC
- Protocol Buffers

## Start

First build:

```powershell
.\gradlew.bat build
```

Start the server:

```powershell
.\gradlew.bat runServer
```

Then start the client in a second terminal:

```powershell
.\gradlew.bat runClient
```

The client sends this name in the Gradle task:

```text
Thomas Micheler
```

## Expected output

Server:

```text
HelloWorld server started on port 50051
Handling hello request for Thomas Micheler
```

Client:

```text
Hello World, Thomas Micheler
```

## Files

### hello.proto

Path:

```text
src/main/proto/hello.proto
```

This file says which method exists.
The method is called `hello`.

The client sends a `HelloRequest`.
The server sends back a `HelloResponse`.

Important part:

```proto
service HelloWorldService {
  rpc hello(HelloRequest) returns (HelloResponse) {}
}
```

### HelloWorldServer.java

The server is started here.
It runs on port 50051.

Important part:

```java
server = ServerBuilder.forPort(PORT)
        .addService(new HelloWorldServiceImpl())
        .build()
        .start();
```

This creates the server and registers the service class.

### HelloWorldServiceImpl.java

This is where the real answer is made.

The server takes the first name and last name and builds the text.

```java
String text = "Hello World, " + request.getFirstname() + " " + request.getLastname();
```

Then the answer is sent back.

### HelloWorldClient.java

The client connects to:

```text
localhost:50051
```

Then it calls the method `hello` on the server.

It does not call the server class directly.
It uses a gRPC stub.

## What I can say in the presentation

First I start the server.
The server waits for requests.

Then I start the client.
The client sends first name and last name.

The server gets the data, makes a Hello World answer and sends it back.

The structure is defined in the proto file.
Gradle/gRPC generates Java classes from the proto file.

## Problems

- Gradle can have problems if only a JRE is selected instead of a JDK.
- I used Java 17.
- Port 50051 must be free.

## What is gRPC?

gRPC is a way for programs to talk to each other.
For example, a client asks something from a server.

It also works with different programming languages, because the interface is written in the proto file.
From this file, classes can be generated for different languages.

## RPC life cycle

1. Client creates a request.
2. Client calls a method on the stub.
3. gRPC sends the data to the server.
4. Server gets the request.
5. Server handles it.
6. Server sends an answer back.
7. Client prints the answer.

## Protocol Buffers workflow

First you write a `.proto` file.
Messages and services are written there.

Then Gradle generates Java code from it.
These generated classes are used in the client and server.

## Benefits of Protocol Buffers

- smaller than JSON
- faster than many text formats
- clear structure
- works with many programming languages

## When is it not so good?

Not so good if you only need a very simple website or REST API.
Also not ideal if humans should read the data directly, because protobuf is binary.

## Example data types

- string
- int32
- bool
- double

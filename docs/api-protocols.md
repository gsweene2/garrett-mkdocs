# API Protocols

References:

* https://www.redhat.com/architect/apis-soap-rest-graphql-grpc
* https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview
* https://www.digitalocean.com/community/tutorials/http-1-1-vs-http-2-what-s-the-difference
* https://www.codecademy.com/article/what-is-rest

## HTTP/1.1

> top-level application protocol that exchanges information between a client computer and a local or remote web server

What is it? text-based request to a host server; response can be HTML page for example, stylesheets, any text

Defines methods like GET, POST

Not all of the resources are returned to the client in the first call for data

Where is it in network stack? It's the application layer, built on top of the transfer layer (TCP)

What is the network stack? Layers: Application -> Transport -> Network -> Data Link

What's the diff between 1.0 and 1.1? In 1.0, the client had to break and remake the TCP connection with every new request.

What is head-of-line blocking? 

> Since multiple data packets cannot pass each other when traveling to the same destination, there are situations in which a request at the head of the queue that cannot retrieve its required resource will block all the requests behind it.

How can HTTP/1.1 Alleviate HOL blocking? multiple TCP connections

What about compression? gzip can be used to compress data in HTTP messages, but header sent as plain text - and cookies can make this larger.

## HTTP/2

Why? Google's intent to reduce web page load latency

Techniques used? compression, multiplexing, prioritization

What's the significant feature? binary framing later (application layer)

Instead of sending plain text, uses binary framing layer to encapsulate messages in binary format (keeping HTTP semantics like verbs, methods, headers)

APIs still create messages in the conventional HTTP format, but underlying layer converts to binary

How does HTTP/2 alleaviate HOL blocking? establish multiple streams of data within a single TCP connection. Streams consist of multiple messages in request/response format, and messages split until smaller units called fromaes.

What about compression? HTTP2 uses HPACK compression to shrink the size of headers - Huffman coding. HPACK can also only send the indexed values or diff headers in frames bast the first frame.

## SOAP

Simple Object Access Protocol

Can be used by a variety of transport protocols: HTTP, FTP, SMTP

Structure: XML, hierachial

Technically, since built on HTTP, supports GET & POST but in practicality messages are complex (XML) so data is sent with a POST request.

```xml
POST /BobsTickers HTTP/1.1
Host: www.example.org
Content-Type: application/soap+xml; charset=utf-8
Content-Length: 275
SOAPAction: "http://cooltickers.org/soap"

<?xml version="1.0"?>
<soap:Envelope xmlns:soap="http://www.w3.org/2003/05/soap-envelope" xmlns:m="http://www.exampletickers.org">
  <soap:Header>
  </soap:Header>
  <soap:Body>
    <m:GetStockPriceRequest>
      <m:StockName>IBM</m:StockName>
    </m:GetStockPriceRequest>
  </soap:Body>
</soap:Envelope>
```

How does the receiving host know it is a host message? The SOAPAction HTTP header

How does a SOAP application route a request to system internals? Based on the SOAPAction header

## REST

Representational State Transfer

It's an architectural style - think of it as a framework for HTTP requests.

Relies on a standard transport protocol: HTTP

The client and server don't have to know about one another - there's no contract between the two

Data can be JSON, XML, CSV, RSS

> The basic premise is that developers use the standard HTTP methods, GET, POST, PUT and DELETE, to query and mutate resources represented by URIs on the Internet.

## gRPC

Relies on a standard transport protocol: HTTP2

Data format? gRPC uses the Protocol Buffers binary format

> Using Protocol Buffers requires that both the client and server in a gRPC data exchange have access to the same schema definition. By convention, a Protocol Buffers definition is defined in a .proto file.

What is the proto file used for? It's is the dictionary by which data is encoded and decoded form the PB binary format

Why would I use gRPC over REST? Increased performance

What allows grpc to be bidirectional? The HTTP/2 protocol

What's the benefit in being able to return a stream type? Instead of the backend constructing an entire response and sending that response, parts of the response can be sent as they are available. Use case: Stock data

Calls appears to both the sender and the receiver as if it were a local call, rather than a remote one executed over the network. 

## Thrift

RPC Framework

makes it possible to modify the protocol used by a service once the service has been defined

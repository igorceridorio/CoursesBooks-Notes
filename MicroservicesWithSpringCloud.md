# Microservices with Spring Cloud

Udemy Course

## Spring Cloud Configuration - Centralized, Versioned Configuration

### Desired solution for configuration

* Language independent
* Dynamic
* Passive
  * Servers (applications) should do most of the work themselves by self registering.
  
 **Spring Cloud Config** designates a centralized server to serve-up configuration information. Clientes connect over HTTP and retrieve their configuration settings.
 
 ### Profiles
 
 * YAML format can hold multiple profiles in a single file.
 
 **luckyword.yml**
 
 ```
 spring:
  profiles: east
  lucky-word: Clover 
---
spring:
  profiles: west
  lucky-word: Rabbit's Foot
 ```
 
 * Result: Properties described by server become part of client application's environment on Spring Applications.
 * **Spring Cloud Server** exposes properties over simple HTTP interface.
 
 *What if the config server is down?*
 
 Spring Cloud Server should tipically run on several instances. Config Server settings override local settings (provide local fallback settings).
 
 ## Spring Cloud Eureka - Service Discovery
 
 Service Discovery provides a single "lookup" service. Clientes register themselves to discover other registrants. Solutions include Eureka, Consul, Zookeper.
 
 **Eureka** provides a "lookup" server.
  * Multiple copies
  * Copies replicate state of registered services
  
* Client servers send heartbeats to Eureka, Eureka removes services without heartbeats.
* Eureka is made to run on multiple Eureka servers, otherwise you will get many warnings in the log.

```
SERVERS:
 @EnableEurekaServer
 
CLIENTS:
 @EnableDiscoveryClient
```

Eureka server is designed for **multiple instance** use, that's why standalone node will actually warn you when it runs without any peers.
If you shut off Eureka server all of the information of who is who goes away, everything is always on memory.

**Which comes first?**

* Config First Bootstrap: Use Config Server to configure location of Eureka server.
* Eureka First Bootstrap: Use Eureka to expose location to config server.
 * Config server is just another client.
 * Client makes two network trips to obtain configuration.

## Spring Cloud Ribbon - Load Balancer

* Client side load balancer.
* Traditional load balancers are server-side components that distribute the incoming traffic among several servers.

(*Application load balancer*) HTTP, TCP, UDP traffic **--->** LOAD BALANCER **--->** [{Application A}, {Application B}, {Application C}]

* Usually uses some algorithm like Round Robin and doesn't do any processing.
* A client-side LB selects which server to call.

Application (**LB**) **--->** [{Server A LB}, {Server B LB}, {Server C LB}] (*Client-side load balancer*)

With this approach closes, fastest response is found in same region, outage forces call to nearby region.

* Ribbon automatically integrates with Eureka service discovery.

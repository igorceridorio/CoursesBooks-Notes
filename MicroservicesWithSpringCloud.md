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

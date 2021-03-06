# Netflix OSS Study
## Spring Cloud service discovery
Spring Cloud service discovery, you can abstract away the physical location (IP and/or server name) of where your servers are deployed from the clients consuming the service. Service consumers invoke business logic for the servers through a logical name rather than a physical location. also handles the registration and deregistration of services instances as they’re started up and shut down.

## Service Registry - Eureka
Spring Cloud Eureka also comes from Netflix OSS. The Spring Cloud project provides a Spring-friendly declarative approach for integrating Eureka with Spring-based applications. Eureka can be used for self-registration, dynamic discovery, and load balancing.

![image](./images/eureka_architecture.png)

- **Application Service**: is a service provider
- **Application Client**: is a service consumer

Eureka consists of a **_Eureka Server_** and a **_Eureka Client_**. <br>
The **_Eureka Server_** is the registry in which all microservices register their availability. The registration includes service identity and its URLs. <br>
The microservices will also use the **_Eureka client_** for discovering the service instances. (in default setting Eureka Server is also a Eureka Client)

The Eureka server is built with a peer-to-peer data synchronization mechanism.
The runtime state information is not stored in a database, but managed using an in memory cache.

### Heartbeat
When a microservice is started, it reaches out to the Eureka server, and advertises its existence with the binding information. Once registered, the service endpoint sends ping requests to the registry every **30** seconds to renew its lease. If the microservice is not responding periodically (default is **90** seconds). The registered instance on Eureka will be removed. 

### Self-Preservation
>EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE

Self preservation mode be trigger by a larger than expected number of registered clients have terminated their connections ungracefully. It's done to ensure catastrophic network events do not wipe out eureka registry data, and having this be propagated downstream to all clients.

When in self-preservation mode, eureka servers will stop eviction of all instances until either:
1. the number of heartbeat renewals it sees is back above the expected threshold
2. self-preservation is disabled (see below)

Self preservation is enabled by default, and the default threshold for enabling self preservation is > 15% of the current registry size.

## Eureka Tutorials
### Basic
1. [eureka-server-basic](eureka-server-basic/README.md)
2. [application-service-provider](application-service-provider/README.md)
3. [application-client-consumer](application-client-consumer/README.md)

### Server Authentication
1. [eureka-server-authenticating](eureka-server-authenticating/README.md)
2. [application-service-provider-authenticating](application-service-provider-authenticating/README.md)
3. [application-client-consumer-authenticating](application-client-consumer-authenticating/README.md)

### High Availability
1. [eureka-server-ha](eureka-server-ha/README.md)

# Client-Side Load Balancer - Ribbon

## Ribbon Tutorials
### Basic
1. [application-client-consumer-ribbon](application-client-consumer-ribbon/README.md)

### Customizing Configuration
1. [application-client-consumer-ribbon-java-config](application-client-consumer-ribbon-java-config/README.md)
2. [application-client-consumer-ribbon-property-config](application-client-consumer-ribbon-property-config/README.md)

### Ribbon without Eureka
1. [application-client-consumer-ribbon-without-eureka](application-client-consumer-ribbon-without-eureka/README.md)

# Declarative REST Client - Feign

## Feign Tutorials
### Basic
1. [application-client-consumer-feign](application-client-consumer-feign/README.md)

### Customizing Configuration
1. [application-client-consumer-feign-java-config](application-client-consumer-feign-java-config/README.md)
2. [application-client-consumer-feign-property-config](application-client-consumer-feign-property-config/README.md)

### Feign Clients with authentication
1. [application-service-provider-with-api-auth](application-service-provider-with-api-auth/README.md)
2. [application-client-consumer-feign-manual](application-client-consumer-feign-manual/README.md)

## Example Scenario
The example has:
- Eureka Server
- Application Client (an order system)
- Application Service (a user system)

In this scenario the order system need to check some user info through the RESTful API with service discovery.
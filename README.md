Service Registry (Eureka Server)

Overview

The Service Registry is responsible for dynamic service registration and discovery within the Movie Ticket Platform microservices ecosystem.

It eliminates hardcoded service URLs and enables scalable, cloud-native communication between services.


Responsibilities
	•	Service registration
	•	Service discovery
	•	Health tracking
	•	Dynamic instance management
	•	Enable client-side load balancing
	•	Support horizontal scaling


Technology Stack
	•	Spring Boot
	•	Spring Cloud Netflix Eureka Server
	•	Spring Cloud Discovery
	•	REST-based service communication


Why Service Registry?

Without service registry:
	•	Services use hardcoded URLs
	•	Scaling requires manual configuration
	•	Failures are harder to detect
	•	Load balancing becomes complex

With service registry:
	•	Services auto-register
	•	Clients discover services dynamically
	•	Scaling is automatic
	•	Fault tolerance improves


Architecture Role

Eureka Server sits at the center of the microservices ecosystem.

        Identity Service
               |
        Theatre Service
               |
        Booking Service
               |
        Payment Service
               |
            Eureka
               |
         API Gateway

All services:
	•	Register themselves
	•	Send heartbeat to registry
	•	Discover other services dynamically


How It Works
	1.	Service starts
	2.	Service registers itself with Eureka
	3.	Eureka stores service metadata
	4.	Other services query Eureka
	5.	Service instances resolved dynamically


Registration Configuration (Client Example)

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
    register-with-eureka: true
    fetch-registry: true

Each microservice includes:
	•	@EnableDiscoveryClient
	•	Eureka client dependency


Server Configuration

server:
  port: 8761

spring:
  application:
    name: service-registry

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false



High Availability Strategy

For production:
	•	Deploy multiple Eureka instances
	•	Configure peer replication
	•	Avoid single point of failure


Load Balancing

When integrated with Spring Cloud LoadBalancer:
	•	Client fetches available instances
	•	One instance selected per request
	•	Round-robin strategy by default
	•	Supports custom strategies


Fault Tolerance

If service instance fails:
	•	Heartbeat stops
	•	Eureka marks instance DOWN
	•	Instance removed from registry
	•	Traffic routed to healthy instances


Cloud Deployment Strategy

Kubernetes Environment

In Kubernetes:
	•	Native service discovery exists
	•	Eureka is optional
	•	Kubernetes DNS can replace Eureka
	•	For hybrid cloud setups, Eureka remains useful


AWS Deployment
	•	Deployed inside EC2 or EKS
	•	Internal Load Balancer for HA
	•	CloudWatch monitoring
	•	Auto Scaling Group for resilience


Azure Deployment
	•	Deployed inside AKS
	•	Internal Load Balancer
	•	Azure Monitor integration
	•	Multi-zone redundancy


CAP Theorem Perspective

Eureka prioritizes:
	•	Availability
	•	Partition Tolerance

It sacrifices strong consistency in distributed environments to maintain service uptime.


Monitoring
	•	/actuator/health
	•	/actuator/info
	•	Eureka dashboard at /

Dashboard shows:
	•	Registered services
	•	Instance status
	•	Availability
	•	Heartbeat timing


Security Considerations

For production:
	•	Protect Eureka dashboard
	•	Enable HTTPS
	•	Restrict access via internal network
	•	Add authentication layer if needed


Production Enhancements
	•	Multi-node cluster
	•	Peer replication
	•	TLS encryption
	•	Monitoring and alerting
	•	Circuit breaker integration


When Not Needed

If fully deployed in Kubernetes:
	•	Kubernetes DNS can replace Eureka
	•	Service mesh (Istio/Linkerd) can handle discovery
	•	Internal ClusterIP services provide dynamic resolution


Architectural Importance

The Service Registry:
	•	Removes tight coupling
	•	Enables scalability
	•	Supports dynamic infrastructure
	•	Improves system resilience
	•	Simplifies microservice communication

It acts as the discovery backbone of the distributed system.

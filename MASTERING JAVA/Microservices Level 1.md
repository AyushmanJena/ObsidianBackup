https://www.youtube.com/playlist?list=PLqq-6Pq4lTTZSKAFG6aCDVDP86Qx4lNas
# Microservices – Level 1 Notes

Microservices is a different way of building applications.  
It is different from traditional **Monolithic architecture**.

It follows the same coding standards (Java, Spring Boot, REST APIs),  
but has a completely different **runtime and deployment model**.

## Monolithic vs Microservices

### Monolithic

- Single codebase
- Single deployment unit
- Single database (usually)
- Complexity is hidden inside the application

### Microservices

- Multiple small services
- Each service is deployed independently
- Each service typically has its own database
- Complexity exists **between services**

## Managing Microservice Complexity
To manage communication and coordination between services, we use:
### 1️. Patterns

- Service Discovery (e.g., Netflix Eureka)
- API Gateway
- Circuit Breaker
- Load Balancing

### 2️. Technologies

- Spring Boot
- Spring Cloud
- REST Clients (WebClient)

# Service-Oriented Architecture (SOA) vs Microservices
### Microservices

- Designed for a **specific problem**
- Focused on a single responsibility
- Not designed for reuse across multiple systems

Example:  
Shopping Cart Service for one application only.

### SOA

- Designed for reuse across multiple systems
- Broader business capability

Example:  
A service that takes IP address and returns location.

# Application Example

## Movie Catalog API

We build 3 microservices:

### 1️. Movie Catalog Service

- Input: `userId`
- Output: List of movies with full details

### 2️. Movie Info Service

- Input: `movieId`
- Output: Movie details

### 3️. Rating Data Service

- Input: `userId`
- Output: List of movie IDs and ratings

### Flow

```
Rating Data Service  ----\
                           \
                            -> Movie Catalog Service -> Client
                           /
Movie Info Service  ------/
```

Movie Catalog Service calls:

- Movie Info Service
- Rating Data Service  
    Then combines data and returns final response.

---

## Can client directly call all microservices?

YES.
But:
- Client must handle multiple calls
- More network overhead
- Tighter coupling

Usually, we prefer aggregation inside backend.

## Important Notes

- Each microservice runs on a different port.
- Each Spring Boot app runs its own embedded Tomcat server.
- Each microservice should focus on a single area of concern.

---

# Calling One Microservice from Another

We use REST calls.
Options:

- RestTemplate (Deprecated)
- WebClient (Recommended)

## Using WebClient (Modern Way)

```java
@Autowired
private WebClient.Builder webClientBuilder;

@RequestMapping("/{userId}")
public List<CatalogItem> getCatalog(@PathVariable String userId) {

    List<Rating> ratings = Arrays.asList(
        new Rating("1234", 4),
        new Rating("5678", 3)
    );

    return ratings.stream().map(rating -> {

        Movie movie = webClientBuilder.build()
            .get()
            .uri("http://localhost:8082/movies/" + rating.getMovieId())
            .retrieve()
            .bodyToMono(Movie.class)
            .block();

        return new CatalogItem(
            movie.getName(),
            "Desc",
            rating.getRating()
        );

    }).collect(Collectors.toList());
}
```
- `build()` → creates WebClient instance
- `get()` → HTTP GET method
- `uri()` → target URL
- `retrieve()` → execute request
- `bodyToMono()` → converts response to Mono
- `block()` → makes it synchronous

Note: If you use `.block()`, it behaves like RestTemplate (blocking).

---

# Why Avoid Returning List as Root in APIs?

Bad:

```json
[
  { ... },
  { ... }
]
```

Better:

```json
{
  "items": [ ... ],
  "totalCount": 10
}
```

Reason:

- Allows adding metadata later
- Prevents breaking changes
- Easier API evolution

# Concurrency

Q: Do we need to manually handle multi-threading?  
Answer: No.
- Each HTTP request gets its own thread.
- RestTemplate and WebClient are thread-safe.
- One request does not affect another.

# External APIs
Yes, microservices can call:
- Internal services
- External APIs (e.g., movie database API)
Very common in real-world systems.


# Security Between Microservices
Two common ways:
1️. HTTPS  
2️. Authentication (Basic Auth, JWT, OAuth)

# Service Discovery

Problem with hardcoded URLs:
- Code changes required
- Cloud environments use dynamic IPs
- Manual load balancing
- Different URLs for Dev/QA/Prod

## Service Discovery Pattern

Instead of:
```
Service A -> http://localhost:8082
```

We use:
```
Service A -> Discovery Server -> Service B
```

The discovery server acts like a phonebook.

## Client-Side Service Discovery

Used by Spring Cloud

Flow:
1. Service asks discovery server for service URL
2. Discovery server responds
3. Client directly calls the service

---

## Server-Side Service Discovery

Flow:
1. Client asks discovery server
2. Discovery server forwards request to service

# Eureka

Netflix Eureka is a service registry used in microservices architecture.

It allows services to:
- Register themselves
- Discover other services
- Avoid hardcoded URLs

## Alternatives to Eureka

- HashiCorp Consul
- Kubernetes native service discovery

Older Netflix stack tools:
- Ribbon (Load balancing)
- Hystrix (Circuit breaker)
- Zuul (API Gateway)

---

# Eureka Architecture

- Discovery Server → Eureka Server
- Microservices → Eureka Clients

Even Eureka Server internally behaves like a client.

# Steps to Make Eureka Work

1️. Start Eureka Server  
2️. Register microservices (clients)  
3️. Discover services using service name

# Starting Eureka Server

Add dependency:
```
spring-cloud-starter-netflix-eureka-server
```

Enable server:

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

application.properties:
```properties
server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

Access dashboard:
```
http://localhost:8761
```

# Enabling Eureka Client

Add dependency:
```
spring-cloud-starter-netflix-eureka-client
```

Enable client:
```java
@SpringBootApplication
@EnableEurekaClient   // optional in newer versions
public class MovieCatalogServiceApplication {
}
```

application.properties:
```properties
spring.application.name=movie-service
server.port=8081

eureka.client.service-url.defaultZone=http://localhost:8761/eureka
```


# Can We Fetch All Registered Services at Runtime?
YES.
Eureka provides APIs to:
- Get all registered applications
- Get instances of a particular service
- Access metadata

# Final Improved Surface-Level Understanding

Microservices:
- Small, focused services
- Independently deployable
- Communicate over HTTP
- Use service discovery
- Avoid hardcoded URLs
- Prefer loose coupling
- Accept distributed complexity

The goal is:

> Build systems that scale independently, deploy independently, and fail independently.
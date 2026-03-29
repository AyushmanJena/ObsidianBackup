### REST API Versioning in Spring Boot 4
- Existing REST clients cannot upgrade immediately
- Spring Boot 4 provides built in API versioning support to support change without breaking existing clients

Ex : Removing or changing fields can break existing clients
Multiple client versions may exist at same time (phased approach)
We need an easy way to run multiple API versions side by side

Use Case : 
```
/api/v1/hello
/api/v2/hello
/api/v3/hello

one logical endpoing : /hello
Three versions... version comes from request path
```
Old clients continue using v1
New Clients move to v2 or v3
Backend selects the correct version automatically

Development : 
1. Develop REST Controller with version support
```java
@RestController
public class HelloWorldController{
	@GetMapping(path="/api/{version}/hello", version = "1")
	public String helloV1(){
		return "Hello v1";
	}
	
	@GetMapping(path="/api/{version}/hello", version = "2")
	public String helloV2(){
		return "Hello v2";
	}
	
	@GetMapping(path="/api/{version}/hello", version = "3")
	public String helloV3(){
		return "Hello v3";
	}
}
```

2. Configure Path Segment versioning in application.properties
application.properties
```
spring.mvc.apiversion.use.path-segment=1
```
- here 1 is the index of the path i.e. /api is 0 and version is 1 and /hello is 2
- The placeholder {version} could be anything


How routing works : 
- Spring extracts the version from URL
- Version is parsed automatically  (skip the 'v', etc.) (it can be anything too)
- Spring matches the request to correct request mapping



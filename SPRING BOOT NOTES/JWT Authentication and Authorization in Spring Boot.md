JWT THEORY notes 

#### SOME SPRING SECURITY NOTES : 
###### CSRF Token :
when you do any update requests like PUT, POST, DELETE where you are changing some data, you need to send CSRF Token

In header :
key = X-CSRF-TOKEN
value = value generated from "/csrf-token"

```java
@GetMapping("/csrf-token")
public CsrfToken getCsrfToken(HttpServletRequest request){
	return (CsrfToken) request.getAttribute("_csrf");
}
```
or inspecting the page and finding the hidden csrf element in browser


46:30 continue



user logs in -> we give them a jwt token (after authentication)
for further requests he goes with the jwt token and not the username and password for every calls.

1. Implement Spring Security

using JJWT : Java JWT 
Dependency : 
```xml
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-api</artifactId>
	<version>0.12.5</version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-jackson</artifactId>
	<version>0.12.5</version>
</dependency>
<dependency>
	<groupId>io.jsonwebtoken</groupId>
	<artifactId>jjwt-impl</artifactId>
	<version>0.12.5</version>
	<scope>runtime</scope>
</dependency>
```

the three dependencies are : 
1. jjwt api
2. jjwt with jackson for json processing
3. jjwt implementation during runtime



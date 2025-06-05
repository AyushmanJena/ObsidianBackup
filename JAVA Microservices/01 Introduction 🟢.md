Microservices is a different way of building Applications.
Different from the traditional Monolithic code base.

It follows the same coding standards but completely different runtime and deployment.

We will build 4 microservices, make them communicate with each other.

Each Microservice is a spring boot application
Microservices typically have their own databases

Monolithic -> Complexity hidden within
Microservices -> Complexity between microservices

Managing this Complexity : 
Patterns -> Make microservices work well together (e.g. Eureka server)
Technologies -> Libraries and frameworks to solve common problems

Eureka Server is a service registry used in microservices architectures to help services discover each other

Service oriented apps vs microservices? 
Microservices applications can be reused (for different use cases) however it is not designed for that. It is designed with a very specific problem statement in mind. Ex : Only for shopping cart service in our app.

On the other hand service oriented apps are designed for multiple use case. Ex : a service which takes IP address and returns the location.



## Application Example
Movie Catalogue API Application

xyz.com/user -> returns a list of movies user has watched
(movie name, movie description, rating)

xyz.com/api/koushik
```
{
	id: Koushik
	name: Koushik K
	movies[
		{id: 1234, name: sadf, rating: 3}
		{id: 1234, name: sadf, rating: 3}
		...
	]
}
```

We will have 3 microservices
1. Movie Catalogue service
	input : userId
	output : movies list with details
2. Movie info Service
	input : movieId
	output : movie details
3. Rating Data Service
	input : userId
	output : Movie Id's and ratings

Movie Catalogue communicates with both the services and returns the result together. 

Q. Is it possible to have the end client directly call the individual microservices and collect data on the client side ? 
YES

Note: You need to handle the port numbers for multiple applications if running on the same machine

Note: Each spring boot project here runs it's own instance of tomcat


CONTINUE FROM PART 10
https://youtu.be/UBnSkjsJ-ow?feature=shared
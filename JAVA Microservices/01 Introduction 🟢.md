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
Microservices applications can be reused (for different use cases) however it is not designed for that. It is designed with a very specific problem statement in mind. 
Ex : Only for shopping cart service in our app.

On the other hand service oriented apps are designed for multiple use case. 
Ex : a service which takes IP address and returns the location.



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

You have a single project focused on a single area of concern and make it easier to manage and handle.


You will need to make a API call from one service to another.
How to make a REST call from your code?
- Calling REST APIs programatically
- Using a REST Client library
- Spring boot comes with a client already in your classpath - RestTemplate (deprecated, use WebClient instead)


Same Models between microservices?
Abstract classes and interfaces with Libraries are not preferred.
2 or more microservices might need the same model class for a particular task. 
You can create duplicates of the class. 
CONS : Duplicated code
PROS : Each microservice can change the model based on its requirements

But when model is changed in one microservice, it needs to notify other microservices using it to make the changes work. 
Like removing and changing a field in a class. 



```java
@RequestMapping
public List<CatelogItem> getCatalog(@PathVariable('userId'') String userId){
	List<Rating> rating = Arrays.asList(
		new Rating("1234", 3),
		new Rating("5678", 4)
	);
	
	return rating.stream().map(rating ->
		{
			Movie movie = restTemplate.getForObject("https://localhost:8082/movie/" + rating.getMovieId(), Movie.class );
			return new CatalogItem(movie.getName(), "Desc", rating.getRating());
		}
	)
	.collect(Collectors.toList());
}
```

CONTINUE FROM : 
https://www.youtube.com/watch?v=F3uJyeAyv5g
5:18
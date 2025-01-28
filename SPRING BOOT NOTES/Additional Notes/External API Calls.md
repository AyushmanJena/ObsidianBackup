# Public API (No Key)

Dependency : 
webflux

Required Files : 
1. Controller : QuoteController
2. Response : QuoteResponse
3. Service : QuiteService and QuoteServiceImpl
4. RestClientConfig : RestClientConfiguration

Note : The API gives us a random quote with its attributes
### ResponseEntity class
- Response Entity is a class that represents an HTTP response.
- It provides a way to configure the response, including body, headers and status code.
- Ex : 
```java
@GetMapping("/")  
public ResponseEntity<?> home(){  
    return new ResponseEntity<>("hello world", HttpStatus.OK);  
}
```

RestController : 
to get quote : 
```java
@Autowired
Public QuoteService quoteService;

@GetMapping("/")
public ResponseEntity<?> home(){
	String str = "Quote : "+quoteService.getQuote().data.getContent() + " -" + 
	quoteService.getQuote().data.getAuthor();
	return new ResponseEntity<>(str, HttpStatus.OK);
}
```

Response Class : 
- This class holds the data from the JSON file in form of Java Object
`QuoteResponse`
```java
public class QuoteResponse {  
    public int statusCode;  
    public Data data;  
    public String message;  
    public boolean success;  
  
    public class Data{  
        public String author;  
        public String content;  
        public ArrayList<String> tags;  
        public String authorSlug;  
        public int length;  
        public String dateAdded;  
        public String dateModified;  
        public int id;
        public String getContent(){  
            return content;  
        }  
        public String getAuthor(){  
            return author;  
        }  
    }  
}
```

Service
- Does all the work to  get response from the API and get the desired output
- Manages the API key if present and uses it for the call if needed

`QuoteServiceImpl`
```java
@Service
public class ... {
	private static final String API = "https://api.freeapi/..."; // url for api call
	@Autowired
	private RestTemplate restTemplate

	@Override
	public QuoteResponse getQuote(){
		String api = API;
		ResponseEntity<QuoteResponse> response = restTemplate.exchange(api, HttpMethod.GET, null, QuoteResponse.class); 
		// (api call, http method, http request entity (e.g. header) here none, class)
		QuoteResponse body = response.getBody();
		return body;
	}
}
```

Config file
`RestClientConfiguration` 
```java
@Configuration
public class RestClientConfiguration{
	@Bean
	public RestTemplate getRestTemplate(){
		return new RestTemplate();
	}
}
```

RestTemplate : 
- RestTemplate is a spring class used for making synchronous HTTP requests on the client side. 
- It simplifies communication with RESTful services by abstracting the complexities of HTTP communication.

# Calls with request body within java 
```java
// Building the request Body
Map<String, Object> body = Map.of(  
        "contents", List.of(  
                Map.of("parts", List.of(  
                        Map.of("text", prompt)  
                ))  
        )  
);

// Setting headers
HttpHeaders headers = new HttpHeaders();
headers.setContentType(MediaType.APPLICATION_JSON);
```

Needs to be updated
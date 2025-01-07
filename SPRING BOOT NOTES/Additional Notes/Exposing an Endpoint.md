with header
```java
@GetMapping("/randomquote") // with header  
public ResponseEntity<?> randomQuote(){  
    HttpHeaders responseHeaders = new HttpHeaders();  
    responseHeaders.set("by","vulcan"); // can add multiple such values
  
    ArrayList<Quote> quotes = quoteService.createQuotesList();  
    Quote randomQuote = quoteService.getRandomQuote(quotes);  
  
    return ResponseEntity.ok()  
            .headers(responseHeaders)  
            .body(randomQuote);  
}
```
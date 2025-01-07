# Sending Image in Base64
1. Add Attribute String to the Model
2. for sending the image first encode it to Base64 format
```java
File file = new File("path");
String encoded = null;
try{
	FileInputStream fileInputStreamReader = new FileInputStream(file);
	byte[] bytes = new byte[(int)file.length()];
	fileInputStreamReader.read(bytes);
	encoded = new String(Base4.getEncoder().encodeToString(bytes));
	// add the encoded string to model
}
catch(Exception e){
	...
}

return ResponseEntity.status(HttpStatus.OK).body(randomQuote);
```

# Receiving Image in Base64 and converting it
```java
String encoded = response.getBody().getEncodedImage();

byte[] bytes = Base64.getDecoder().decode(encoded);
BufferedImage image = null;

try{
	image = ImageIO.read(new ByteArrayInputStream(bytes));
	ImageIO.write(image, "png", new File("path"));
}
catch(Exception e){
	...
}
```
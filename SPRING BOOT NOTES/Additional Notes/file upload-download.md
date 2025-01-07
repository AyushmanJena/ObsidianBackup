DownloadController.java
```java
@Controller
public class DownloadController {
    private static String UPLOADED_FOLDER = "Storage";

    private static final String DIRECTORY = "Storage";
    private static final String DEFAULT_FILE_NAME = "test.pdf";

    ServletContext  context;
    @Autowired
    public DownloadController(ServletContext context){
        this.context = context;
    }

    @GetMapping("/downloadfile")
    public String index(){
        System.out.println("downloadFile called");
        return "downloadFile";
    }

    @GetMapping("/download")
    public ResponseEntity<InputStreamResource> downloadFile1(@RequestParam(defaultValue = DEFAULT_FILE_NAME) String fileName) throws IOException{
        MediaType mediaType = getMediaTypeForFileName(context, fileName);
        System.out.println("file Name : " + fileName );
        System.out.println("Media Type : " + mediaType);

        File file = new File(DIRECTORY + "/" + fileName);
        InputStreamResource resource = new InputStreamResource(new FileInputStream(file));

        return ResponseEntity.ok()
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment;filename=" + file.getName())
                .contentType(mediaType)
                .contentLength(file.length())
                .body(resource);
    }

    private MediaType getMediaTypeForFileName(ServletContext servletContext, String fileName){
        String mineType = servletContext.getMimeType(fileName);
        try{
            MediaType mediaType = MediaType.parseMediaType(mineType);
            return mediaType;
        }catch (Exception e){
            return MediaType.APPLICATION_OCTET_STREAM;
        }
    }
}

```

UploadController
```java
@Controller  
public class UploadController {  
    private static String UPLOADED_FOLDER = "Storage";  
  
    @GetMapping("/")  
    public String index(){  
        System.out.println("home called");  
        return "upload";  
    }  
  
    @PostMapping("/upload")  
    public String singleFileUpload(@RequestParam("file")MultipartFile file, RedirectAttributes redirectAttributes){  
        if(file.isEmpty()){  
            redirectAttributes.addFlashAttribute("message", "Please select a file to upload");  
            return "redirect:uploadStatus";  
        }  
        try{  
            byte[] bytes = file.getBytes();  
            Path path = Paths.get(UPLOADED_FOLDER + file.getOriginalFilename());  
            Files.write(path, bytes);  
  
            redirectAttributes.addFlashAttribute("message", "you successfully uploaded '"+ file.getOriginalFilename()+ "'");  
        }catch (IOException e){  
            e.printStackTrace();  
        }  
  
        return "redirect:/uploadStatus";  
    }  
  
    @GetMapping("/uploadStatus")  
    public String uploadStatus() {  
        return "uploadStatus";  
    }  
}
```

HTML thymeleaf TEMPLATES : 

downloadFile.html
```html
<!DOCTYPE html>  
<html xmlns:th="http://www.thymeleaf.org">  
<body>  
  
<h1>Spring Boot file Download example</h1>  
  
<form method="GET" action="/download" enctype="multipart/form-data">  
    <input type="text" name="fileName" /><br/><br/>  
    <input type="submit" value="Submit" />  
</form>  
<br>  
  
<a href="/">Home</a>  
  
</body>  
</html>
```

upload.html
```html
<!DOCTYPE html>  
<html xmlns:th="http://www.thymeleaf.org">  
<body>  
  
<h1>Spring Boot file upload example</h1>  
  
<form method="POST" action="/upload" enctype="multipart/form-data">  
    <input type="file" name="file" /><br/><br/>  
    <input type="submit" value="Submit" />  
</form>  
<br>  
<br>  
<a href="/downloadfile">Download File</a>  
  
</body>  
</html>
```
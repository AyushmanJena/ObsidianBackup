- using openpdf

1. Dependency 
```xml
		<dependency>
			<groupId>com.github.librepdf</groupId>
			<artifactId>openpdf</artifactId>
			<version>2.0.3</version>
		</dependency>
```

2. Pdf service
```java
@Service  
public class PdfService {    
    public ByteArrayInputStream createPdf(int id, String str1) {   
        String title = "TITLE";  
        String content = "Content of the pdf";  
  
        ByteArrayOutputStream out = new ByteArrayOutputStream();  
        Document document = new Document();  
  
        try{  
            PdfWriter.getInstance(document, out);  
            document.open();  
  
            Font titleFont = FontFactory.getFont(FontFactory.HELVETICA_BOLD, 20);  
            Paragraph titlePara = new Paragraph(title, titleFont);  
            titlePara.setAlignment(Element.ALIGN_CENTER);  
            document.add(titlePara);  
  
            Font paraFont = FontFactory.getFont(FontFactory.HELVETICA, 16);  
            Paragraph paragraph = new Paragraph(content, paraFont);  
            
            // to add content during runtime
            paragraph.add(new Chunk("\nStr 1 : " + str1));  
            document.add(paragraph);  
  
            document.close();

			// to save the pdf 
            String fileName = "report_"+id+".pdf";  
            String directoryPath = "Storage/";
            File file = new File(directoryPath + fileName);  
  
            file.getParentFile().mkdirs();  
  
            try(FileOutputStream fileOut = new FileOutputStream(file)){  
                fileOut.write(out.toByteArray());  
            }  
        }catch (Exception e){  
            e.printStackTrace(e);  
        } 
        
        return new ByteArrayInputStream(out.toByteArray());  
    }  
}
```

3. Using the service
```java
ByteArrayInputStream pdf = pdfService.createPdf(id, text);
```
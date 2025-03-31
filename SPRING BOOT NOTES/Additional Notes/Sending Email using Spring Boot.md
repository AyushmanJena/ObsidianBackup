
Dependency : 
```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-mail -->  
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-mail</artifactId>  
    <version>3.4.1</version>  
</dependency>
```

Properties : 
```properties
# Email Configuration  
spring.mail.host=your_smtp_server_host  
spring.mail.port=your_smtp_server_port  
spring.mail.username=your_email_username  
spring.mail.password=your_email_password  
spring.mail.properties.mail.smtp.auth=true  
spring.mail.properties.mail.smtp.starttls.enable=true
```
Ex : 
```properties
# Email Configuration  
spring.mail.host= smtp.gmail.com  
spring.mail.port= 587  
spring.mail.username= ayushdottwentyfour@gmail.com  
spring.mail.password= xxxx xxxx xxxx xxxx #generated app password in google 
spring.mail.properties.mail.smtp.auth= true  
spring.mail.properties.mail.smtp.starttls.enable=true #data encryption
```
- Port for unencrypted : 25 by default
- for encrypted : 587
- starttls : start transport layer security (for encryption)

EmailService.java
```java
@Service  
public class EmailService {  
  
    @Autowired  
    private JavaMailSender javaMailSender;  
  
    public void sendEmail(String to, String subject, String body){  
        try{  
            SimpleMailMessage mail = new SimpleMailMessage();  
            mail.setTo(to);  
            mail.setSubject(subject);  
            mail.setText(body);  
            javaMailSender.send(mail);  
        }catch (Exception e){  
            e.printStackTrace();  
        }  
    }  
}
```
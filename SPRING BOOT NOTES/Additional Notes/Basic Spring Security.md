Hardcoded usernames and passwords 

##### Dependency for Spring Security : 
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>
```
- Default user : user
- default password : (generated in console)

Overriding default username and password :
`application.properties`
```properties
spring.security.user.name = scott
spring.security.user.password = test123
```

`SecurityConfig.java`
```java
@Configuration  
public class SecurityConfig {  
    @Bean  
    public InMemoryUserDetailsManager userDetailsManager(){  
        UserDetails admin = User.builder()  
                .username("user123")  
                .password("{noop}test123")  
                .roles("ADMIN")  
                .build();  
        return new InMemoryUserDetailsManager(admin);  
    }  
  
    @Bean  
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception{  
        http.authorizeHttpRequests(configurer ->  
                configurer  
                        .requestMatchers("/auth/**").permitAll()  
                        .requestMatchers(HttpMethod.GET, "/admin/**").hasRole("ADMIN")  
                        .requestMatchers(HttpMethod.POST, "/admin/**").hasRole("ADMIN")  
                        .requestMatchers(HttpMethod.PUT, "/admin/**").hasRole("ADMIN")  
                        .requestMatchers(HttpMethod.DELETE, "/admin/**").hasRole("ADMIN")  
                        .anyRequest().permitAll()  
        );  
        http.httpBasic(Customizer.withDefaults());  
        http.csrf(csrf -> csrf.disable());  
        return http.build();  
  
    }  
  
    @Bean  
    public AuthenticationManager authenticationManager(AuthenticationConfiguration authenticationConfiguration){  
        return authenticationConfiguration.getAuthenticationManager();  
    }  
}
```

LoginRequest.java : model
```java  
public class LoginRequest {  
    private String username;  
    private String password;  
  
    public LoginRequest() {  
    }  
  
    public LoginRequest(String username, String password) {  
        this.username = username;  
        this.password = password;  
    }  
  
    public String getUsername() {  
        return username;  
    }  
  
    public void setUsername(String username) {  
        this.username = username;  
    }  
  
    public String getPassword() {  
        return password;  
    }  
  
    public void setPassword(String password) {  
        this.password = password;  
    }  
}
```

AuthController.java
```java
@RestController  
@RequestMapping("/auth")  
public class AuthController {  
  
    @Autowired  
    private AuthenticationManager authenticationManager;  
  
    @PostMapping("/login")  
    public ResponseEntity<String> login(@RequestBody LoginRequest loginRequest, HttpSession session){  
        try{  
            Authentication authentication = authenticationManager.authenticate(  
                    new UsernamePasswordAuthenticationToken(  
                            loginRequest.getUsername(),  
                            loginRequest.getPassword()  
                    )  
            );  
  
            SecurityContextHolder.getContext().setAuthentication(authentication);  
            session.setAttribute(  
                    HttpSessionSecurityContextRepository.SPRING_SECURITY_CONTEXT_KEY,  
                    SecurityContextHolder.getContext()  
            );  
            return ResponseEntity.ok("Login successful");  
        } catch(AuthenticationException e){  
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).body("Invalid credentials");  
        }  
    }  
  
    @PostMapping("/logout")  
    public ResponseEntity<String> logout(HttpSession session){  
        session.invalidate();  
        SecurityContextHolder.clearContext();  
        return ResponseEntity.ok("Logged out successfully");  
    }  
}
```


# Integrating Database Support 
Need to create table and insert admin manually for this 

Admin.java
```java
package com.example.demo.entity;

import jakarta.persistence.*;

@Entity
@Table(name = "admins")
public class Admin {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String username;

    @Column(nullable = false)
    private String password;

    public Admin() {
    }

    public Admin(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public Long getId() {
        return id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }
}
```

AdminRepository.java
```java
package com.example.demo.repository;

import com.example.demo.entity.Admin;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface AdminRepository extends JpaRepository<Admin, Long> {

    Optional<Admin> findByUsername(String username);
}
```

CustomUserDetailsService.java
```java
package com.example.demo.security;

import com.example.demo.entity.Admin;
import com.example.demo.repository.AdminRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.*;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class CustomUserDetailsService implements UserDetailsService {

    @Autowired
    private AdminRepository adminRepository;

    @Override
    public UserDetails loadUserByUsername(String username)
            throws UsernameNotFoundException {

        Admin admin = adminRepository.findByUsername(username)
                .orElseThrow(() ->
                        new UsernameNotFoundException("Admin not found"));

        return new User(
                admin.getUsername(),
                admin.getPassword(),
                List.of(new SimpleGrantedAuthority("ROLE_ADMIN"))
        );
    }
}
```

SecurityConfig.java
```java
package com.example.demo.config;

import org.springframework.context.annotation.*;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.authentication.configuration.AuthenticationConfiguration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {

        http.authorizeHttpRequests(configurer ->
                configurer
                        .requestMatchers("/auth/**").permitAll()

                        .requestMatchers(HttpMethod.GET, "/admin/**")
                        .hasRole("ADMIN")

                        .requestMatchers(HttpMethod.POST, "/admin/**")
                        .hasRole("ADMIN")

                        .requestMatchers(HttpMethod.PUT, "/admin/**")
                        .hasRole("ADMIN")

                        .requestMatchers(HttpMethod.DELETE, "/admin/**")
                        .hasRole("ADMIN")

                        .anyRequest().permitAll()
        );

        http.httpBasic(Customizer.withDefaults());

        http.csrf(csrf -> csrf.disable());

        return http.build();
    }

    @Bean
    public AuthenticationManager authenticationManager(
            AuthenticationConfiguration authenticationConfiguration)
            throws Exception {

        return authenticationConfiguration.getAuthenticationManager();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```


TABLE CREATION AND ADMIN INSERT : 
```sql
CREATE TABLE admins (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL
);

INSERT INTO admins(username, password)
VALUES (
    'admin',
    '$2a$10$zP8....'
);
```
First convert the normal password to Bcrypt using step size 10 (default for spring boot)
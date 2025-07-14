By Chai aur code
https://youtu.be/xrj3zzaqODw?feature=shared

JWT is a stateless mechanism
- Its state is not stored in any database or file
- who has the token is authorized 

### What is JWT ? 
JSON Web Token 
random collection of letters and numbers, a long string
3 components separated by .

Ex:
`JzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`

JSON web Tokens are an open source, industry standard RFC 7519 method for representing claims securely between two parties

Various encoding algorithms are used for encryption
Most common is HS256

3 parts of  jwt token : 
1. first parts determines which encryption algorithm is being used
2. second part : the actual information
3. third part : signature or payload data(checking correctness)

iat -> issued at
it can also hold data about expiry time



Authentication vs Authorization 
- Authentication : Unique signature, when signing up or creating account 
- Authorization : Access to resources 

accessing multiple resources with the key


![[Pasted image 20250601000450.png]]



### How do you securely store a JWT on the Client Side
- JWT acts as a short lived token

1. storing in localstorage (can be extracted)
2. session storage
3. cookies with flag (configuring to not be accessible through js)

- best suggestion is to expire them as soon as possible
- refresh token by requesting a new one after lets say 10 minutes


### What are the common use cases for JWT ? 
- Authentication
- Authorization 
- Information Exchange (through payload)

### How can you invalidate a JWT
- With every implementation of JWT you get invalidation option in some way or other
npm, django etc. all have the option to expiry 

Refresh token : 
explained below

### THE PROCESS: 
client sends authentication data to server
server returns a jwt token
now everytime the client makes a call it will attach the jwt token with the call for authorization of resources

Refreshing of token : 
after access token expires
the access token is renewed 
- it is stored in the database
- it is stateful mechanism
there will be two tokens : access token and refresh token
- the refresh token will be stored in the database
- after the access token is expired, through the stored refresh token the access token is renewed

- JWT tokens can be blacklisted and whitelisted if stored in the database

### JWT Tokens vs Sessions
Three tier architecture
Client-server-database

##### JWT TOKEN (stateless mechanism)  :
![[Pasted image 20250601002937.png]]

![[Pasted image 20250601003541.png]]
again the first step repeats


##### Sessions (stateful)
- we need to store something in the db and verify it everytime
- consumes more server memory due to the db read write use

![[Pasted image 20250601003940.png]]
(cropped -> Invalid session response)



To practise with jwt tokens go to freeapi.app -> authentication



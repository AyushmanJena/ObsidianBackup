Overview : 
- JWT -> Encoded json decument
- When the backend first validates the username and password, it creates back a JWT.
- Encoded with a secret key only known to the backend
- Now the frontend must store the JWT and use it for all the following requests
- As the secret key is only known by the backend, if the received JWT is correctly decoded, the backend assumes the JWT was created by itself and it can trust the request.
- As the data stored in frontend is vulnerable, it must not contain any critical information and it should have a expiry time

NOTE : This Project uses Axios and not HTTP Client
### SPRING BOOT AND ANGULAR


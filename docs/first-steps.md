# First steps

Our API adheres to REST best practices, ensuring a smooth and efficient integration process.

## Step 1: Register Your Application

To begin using our API, your application needs to be set up. This involves registering your application with our backend and configuring the necessary endpoints to interact with our services.

Contact our support team to register your application. Provide details about your company and the intended use of the API. Once registered, you will receive an API key and other credentials needed for authentication.

## Step 2: Authentication

Our API employs token-based authentication. Each request is signed with an `hmac` signature that also includes a timestamp to prevent tampering and replay attacks.

There is a whole section on [authentication](?p=/authentication), but requests have the following format in general:

```http
GET /v3/igr/dub/foo/bar/receive?expire=5&recid=00001 HTTP/1.1  
Host: api.instantcmr.com
x-icmr-auth-1: oh91tDqJySK8wur2V6ZNhg 20171123.231834.311 d374ad26-6f8e-4d72-9004-4c713409bacd - cCalf3gwUOFaiLsTHWJSShGWem4cuyTFmFkquhzAbes=
Content-Type: application/json
````

## Step 3: Consuming the API

With the endpoints configured and authentication in place, you can start consuming our API. Here are some examples of common requests you might make:

### Create a trip and assign it to a user

```http
PUT /v3/igr/trip/{copid}/{tripxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
[If-Match: {ETag}]  
[If-None-Match: "*"]  
Content-Length: 1779  

{tripeu}
```

### Send a message to a chatroom

```http
PUT /v3/igr/room/{copid}/{roomxtid}/post/{postxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
If-Match: {etagpost}  
Content-Length: 1779  

{posteu}  
```

### Upload Document

Upload a document to a user’s document storage

```http
PUT /v3/igr/dbox/{copid}/{userxtid}/fil/{file-path} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/octet-stream  
Accept: */*  
Content-Length: 11741  

{document-bytes} 
```

## Step 4: Testing and Debugging

Before deploying your integration to production, thoroughly test all endpoints and handle any potential errors gracefully. 

- **Rate Limiting**: Be mindful of rate limits to avoid throttling. Make efficient use of API calls.
- **Error Handling**: Implement robust error handling to manage different types of errors such as authentication failures, invalid inputs, and server errors.

By following these steps and best practices, you can successfully integrate our API into your system, enhancing your logistics operations with the powerful features our platform offers.

For further assistance, please refer to our detailed API documentation or contact our support team. We are here to help you every step of the way.

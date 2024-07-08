
# Entity Versioning with ETag

The instantCMR API uses the HTTP headers **If-Match** and **ETag** to manage conditional requests, particularly for handling concurrent updates and ensuring data consistency. Let's discuss these concepts through user account management, but it applies to other entity types as well.

To create a new instantCMR user account, send a [PUT user](?p=/user-api#put-user) request. Make sure that you generate a globally unique **userxtid** for the entity. Send the request with an **If-None-Match: \*** http header to avoid overwriting an existing user entity.

```http 
PUT /v3/igr/user/{copid}/{userxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
If-None-Match: *
Content-Length: 1779

{usereu}
```

**[Usereu](?p=/user-schemas#user-update-usereu)** in the payload consists the user's name and other associated metedata and should comply to the user update schema. 

The system returns the following reply.

```http
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag}  
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{usered}  
```

Subsequents [PUT user](?p=/user-api#put-user) requests to the same url (i.e. using the same **userxtid**) should containt an **If-Match: {ETag}** http header to avoid overwriting any updates made by other endpoints or mobile devices. The data sent in the request will fully replace the data of the previous version of the user entity being updated. This also means that any data you skip from the request body of the update request will be removed from instantCMR.

```http 
PUT /v3/igr/user/{copid}/{userxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
If-Match: {ETag}
Content-Length: 1779

{usereu}
```

The request will succeed if

*   The user entity version provided in the **If-Match** header matches the current entity version stored by instantCMR, or
*   The user entity sent in the request body matches exactly the current entity stored by instantCMR.

When the requests succeeds the response body contains the user entity after the update and the **ETag** http response header will contain the user entity version after the update. If none of the above conditions are met, the update will be rejected and the response will contain a **412 PreconditionFailed** http status code.

#### Conflict avoidance

To ensure that lost updates are avoided instantCMR requires that all [PUT user](?p=/user-api#put-user) requests are accompanied with either an **If-Match** (for entity updates) or an **If-None-Match** (for entity creations) header. If the **ETag** in these headers does not match the current entity to be updated the API returns a **412 PreconditionFailed** http status code.

To resolve any potential conflicts or lost updates, send a [GET user](?p=/user-api#get-user) request with the same **userxtid** and retrieve the current state of the entity and its corresponding **ETag**. Then apply the changes you originally wanted to make on the retrieved entity, potentially merging your changes with changes made by others. In case of conflicts (e.g. updates to the same field of the entity) it is also your responsibility to provide a resolution by either

*   Dropping your changes
*   Dropping changes made by others
*   Combining both changes into one update.

You should then re-send the [PUT user](?p=/user-api#put-user) request with the merged user entity and the latest **ETag** you received in the **If-Match** request header. The update should typically succeed, but should it not, repeat the [GET user](?p=/user-api#get-user), merge, resolve conflicts [PUT user](?p=/user-api#put-user) process until the system arrives at a fixpoint.

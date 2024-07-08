# User API
The User API enables integration clients to create, activate, deactivate or configure user accounts of the instantCMR service, give or revoke permissions, provide access to the instantCMR Mobile App and the instantCMR Hub.

### Stable identity

The user account identifier (**userxtid**) is a globally unique **userxtid** for the entity. The user account identifier can not be updated. This is also true after the user account has been deleted. Sending a [PUT user](?p=/user-api#put-user) request to a previously deleted user account will undelete the user account and modify its data to the user entity sent in the request body.

### Deactivating users

A user account can be deactivated by updating its **ofDeleted** field to **true**, or by sending a [DELETE user](?p=/user-api#delete-user) request. Deactivated users will be unable to login to the instantCMR Hub or submit documents and receive trip updates on their mobile devices.

instantCMR will keep all historical data of deactivated users and make them available for examination on the instantCMR Hub.

A previously deactivated user account can be reactivated by removing its **ofDeleted** field.

### Roles

By assigning roles to a user account you can control what the user of the account will have access to within instantCMR.

Users with a _driver_ role can use the instantCMR Driver App on their mobile devices to receive documents and trip assignments and to submit scans of documents and trip statuses. In order to do so they must have one or more mobile devices assigned to them on the instantCMR hub. To assign driver role to a user account, set the **roles.odriver** field of the user entity to a valid [Driver Role Entity](?p=/user-schemas#driver-role-driverrole). You can remove this role from the user account by removing the **roles.odriver** field from the user entity.

Users with the _dispatcher_ role can use the instantCMR Hub to send documents to drivers on the Documents tab and query document scans submitted by drivers on the Inbox tab. Dispatchers can only access drivers within the same organization unit (identified by their **ouxtid** field). To assign dispatcher role to a user account, set the **roles.odisp** field of the user entity to an empty object (**{}**). You can remove this role from the user account by removing the **roles.odisp** field from the user entity.

Users with the _reviewer_ role can use the instantCMR Hub to query and drivers progress with their assigned trips and review or move document scans submitted by drivers to their assigned trips on the Trips tab. Reviewers have access to all trips regardless of the organization unit they belong to. To assign reviewer role to a user account, set the **roles.orev** field of the user entity to an empty object (**{}**). You can remove this role from the user account by removing the **roles.orev** field from the user entity.

Users with the _device inventory administrator_ role can use the instantCMR Hub to access user accounts and the mobile device inventory and to manage assignment of mobile devices to driver accounts. They can do so on the Users and Devices tabs. Device inventory administrators can only access drivers, that is user accounts with a driver role and no other role. To assign device inventory administrator role to a user account, set the **roles.odia** field of the user entity to an empty object (**{}**). You can remove this role from the user account by removing the **roles.odia** field from the user entity.

Users with the _chat editor_ role can create, add members to, remove members from, mute and unmute members of any chat group within their organization unit that they are a member of. They can do so on the Messages tab of instantCMR Hub. To assign chat editor role to a user account, set the **roles.ochedit** field of the user entity to an empty object (**{}**). You can remove this role from the user account by removing the **roles.ochedit** field from the user entity.

Users with the _chat administrator_ role can create, add members to, remove members from, mute and unmute members of any chat group within their organization unit. They can also search for, read and post messages into chat groups that they are not a member of. They can do so on the Messages tab of instantCMR Hub. To assign chat administrator role to a user account, set the **roles.ochadmin** field of the user entity to an empty object (**{}**). You can remove this role from the user account by removing the **roles.ochadmin** field from the user entity.

Users with the _integration endpoint_ role can use the instantCMR API to manage trips, user accounts and their assignments and to subscribe to updates on trip, user and submitted documents. You cannot assign or unassign integration endpoint role to a user account via the instantCMR API and attempting to do so will yield a **403 Forbidden** http status code. Updating a user account that has integration endpoint role via the instantCMR API is also forbidden. Should you need further user accounts with integration endpoint access please contact our customer support at **help@instantcmr.com**.

### instantCMR Hub accounts

Users with access to the instantCMR Hub will need a login name to log in. The login name consists of an account name (**oaccn**), the ‘**@**’ (at) symbol and the company id (**copid**) of the user entity.

Example: if the company id is **LogisticsGmbh** and the user’s account name is **bertram.friedrich** then the user can login on the instantCMR Hub with the login name **betram.friedrich@LogiscticsGmbh**.

To provide an account name for a user account, set the **oaccn** field to the account name. Note that you can not set the company id as it is set by our customer support when creating a company account for you.

The provided account name must be globally unique and may consist only of letters, digits, the ‘**.**’ (dot) and ‘**\-**’ (dash) characters. Sending a [PUT user](?p=/user-api#put-user) request with a clashing account name or containing invalid characters will result in a **400 BadRequest** http status code.

If you send a [PUT user](?p=/user-api#put-user) request for a user having access to the instantCMR Hub without providing an account name (**oaccn**), an account name will be generated for the user based on its user name (**usern**) field. Should the generated account name collide with another account name the request will yield a **400 BadRequest** status code.

### Email contacts
You can set up various email addresses that instantCMR uses to send incoming document notifications, password reset emails and account invite emails to. Please note that all email addresses must pass the pre-authorization check in order to the [PUT user](?p=/user-api#put-user) request succeed. If any email addresses included in the user entity fail the pre-authorization checks, the request will fail with a **400 BadRequest** http status code.

See [Anti-Spam requirements for email contacts](?p=/anti-spam#anti-spam-requirements-for-email-contacts) for further details.

### Document storage
Each user account is provided with a document storage area that can be used by dispatchers to share documents with drivers or other instantCMR user accounts. This is true even for user accounts with no further roles assigned.

In order to subscribe a user’s account to another user’s document storage, include the **userxtid** of the sharing user into the **dboxc.rguserxtidFollow** field of the subscribing user account’s user entity. All documents uploaded to the sharing user’s document storage (or any document storage the sharing user is subscribed to) will also appear in the subscriber user’s document storage both on instantCMR Hub and the user’s mobile devices.


## GET user

Retrieves a user entity identified by **userxtid**.

### Request
```http 
GET /v3/igr/user/{copid}/{userxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag} 
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{usered}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response body contains the user entity.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT user](?p=/user-api#put-user) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **usered**
* [usered](?p=/user-schemas#user-response-usered)
* User response entity.
{% /table %}



## PUT user

Stores a user entity, optionally replacing any previous entity stored under **userxtid**.

### Request

To create a new instantCMR user account, send a [PUT user](?p=/user-api#put-user) request to the url below, making sure that you generate a globally unique **userxtid** for the entity. Send the request with an **If-None-Match: \*** http header to avoid overwriting an existing user entity.

To update an existing user account, send a [PUT user](?p=/user-api#put-user) request to the same url used to create the user entity (i.e. using the same **userxtid**) with the updated data in the request body. Send the request with an **If-Match: ETag** http header to avoid overwriting any updates made by other endpoints or mobile devices. The data sent in the request will fully replace the data of the previous version of the user entity being updated. This also means that any data you skip from the request body of the update request will be removed from instantCMR.

```http 
PUT /v3/igr/user/{copid}/{userxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
Content-Type: application/json  
[If-Match: {ETag}]  
[If-None-Match: *]  
Content-Length: 1779  

{usereu}
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **ETag**
* Entity version tag. Provide previous user entity version tag when updating an already existing user. When creating a new user pass **‘\*’** in the **If-None-Match** header.
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
---
* **usereu**
* [User update entity](?p=/user-schemas#user-update-usereu).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag}  
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{usered}  
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains updated user entity in the response body and updated user entity version tag in the **ETag** response header.
---
* 400 BadRequest
* Malformed request or API quota violation. See also [Quotas and Limits](?p=/trip-api#quotas-and-limits) and [Anti-Spam requirements for email contacts](?p=/anti-spam#anti-spam-requirements-for-email-contacts).
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found. Only returned when updating an existing entity (i.e. **If-Match** header is present).
---
* 412 PreconditionFailed
* **ETag** specified in **If-Match** or **If-None-Match** headers did not match the current user entity version. Retrieve the current user entity using [GET user](?p=/user-api#get-user), reapply the changes, and retry the [PUT user](?p=/user-api#put-user) request with updated **ETag**.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}
### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT user](?p=/user-api#put-user) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **usered**
* [usered](?p=/user-schemas#user-response-usered)
* User response entity.
{% /table %}



## DELETE user

Deactivates a user account identified by **userxtid**.

### Request

The [DELETE user](?p=/user-api#delete-user) request is equivalent to sending a [PUT user](?p=/user-api#put-user) request that only updates the user entity by adding the field **ofDeleted** with a value of **true**. The only difference is that the [DELETE user](?p=/user-api#delete-user) request is guaranteed to preserve any other data of the user entity and thus requires no **ETag** negotiation as is the case with [PUT user](?p=/user-api#put-user).

```http 
DELETE /v3/igr/user/{copid}/{userxtid} HTTP/1.1  
Host: api.instantcmr.com  
x-icmr-auth-1: {authentication-token}  
```

{% table %}
---
* **copid**
* Company identifier assigned by instantCMR
---
* **userxtid**
* Client-generated user identifier
---
* **authentication-token**
* See [Authentication](?p=/authentication#authentication).
{% /table %}

### Response
```http 
HTTP/1.1 200 OK  
Date: Thu, 23 Jan 2020 16:29:47 GMT  
Content-Type: application/json; charset=UTF-8  
ETag: {ETag}
Content-Length: 1386  
Server: Jetty(9.2.1.v20140609)  

{usered}
```

### Response codes
{% table %}
---
* 200 OK
* Request was successful. Response contains updated user entity in the response body and updated user entity version tag in the **ETag** response header.
---
* 401 Unauthorized
* Missing or invalid **authentication-token**.
---
* 403 Forbidden
* Authenticated user does not have sufficient permission to access users of the company designated by **copid**.
---
* 404 Not Found
* User with id of **userxtid** not found.
---
* 429 Too Many Requests
* Request has been throttled as user exceeded API quota.
{% /table %}

### Response headers
{% table %}
---
* **ETag**
* string
* Entity version tag. Must be supplied in subsequent [PUT user](?p=/user-api#put-user) requests in the **If-Match** header.
{% /table %}

### Response body
{% table %}
---
* **usered**
* [usered](?p=/user-schemas#user-response-usered)
* User response entity.
{% /table %}


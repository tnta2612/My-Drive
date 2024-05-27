# Discovering API documentation

Look for endpoints that may refer to API documentation, for example:
- `/api`
- `/swagger/index.html`
- `/openapi.json`

If we identify an endpoint for a resource, make sure to investigate the base path. For example, if we identify the resource endpoint `/api/swagger/v1/users/123`, then we should investigate the following paths:
- `/api/swagger/v1`
- `/api/swagger`
- `/api`

We could also use a list of common paths to directly fuzz for documentation.
Also look out for JavaScript files. These can contain references to API endpoints (Use JS Link Finder BApp).

# Identifying supported content types

API endpoints often expect data in a specific format. They may therefore behave differently depending on the content type of the data provided in a request. Changing the content type may enable us to:

- Trigger errors that disclose useful information.
- Bypass flawed defenses.
- Take advantage of differences in processing logic. For example, an API may be secure when handling JSON data but susceptible to injection attacks when dealing with XML.

To change the content type, modify the `Content-Type` header, then reformat the request body accordingly. 

> [!tip] We can use the [Content type converter](https://portswigger.net/bappstore/db57ecbe2cb7446292a94aa6181c9278) BApp to automatically convert data between XML and JSON.

> [!tip] For the lab:
> We might be able to use a PATCH request to change the price of a product

# Fuzzing to find hidden endpoints

Once we have identified some initial API endpoints, we can fuzz to uncover hidden endpoints. For example, consider a scenario where we have identified the following API endpoint for updating user information: `PUT /api/user/update`

To identify hidden endpoints, we could use Burp Intruder to fuzz for other resources with the same structure. For example, we could fuzz the `/update` position of the path with a list of other common functions, such as `delete` and `add`.

# Identifying hidden parameters

We can often identify hidden parameters by manually examining objects returned by the API.

For example, consider a `PATCH /api/users/` request, which enables users to update their username and email, and includes the following JSON: `{ "username": "wiener", "email": "wiener@example.com", }`
A concurrent `GET /api/users/123` request returns the following JSON: `{ "id": 123, "name": "John Doe", "email": "john@example.com", "isAdmin": "false" }`
This may indicate that the hidden `id` and `isAdmin` parameters are bound to the internal user object, alongside the updated username and email parameters.

# Testing mass assignment vulnerabilities

To test whether we can modify the `isAdmin` parameter value, add it to the `PATCH` request: `{ "username": "wiener", "email": "wiener@example.com", "isAdmin": true, }`
The user `wiener` may be incorrectly granted admin privileges.

# Server-side parameter pollution

Server-side parameter pollution occurs when a website embeds user input in a server-side request to an internal API without adequate encoding.

## Testing for server-side parameter pollution in the query string

Use query syntax characters like `#`, `&`, and `=` in input and observe how the application responds.

Consider a vulnerable application that enables us to search for other users based on their username. When we search for a user, your browser makes the following request: `GET /userSearch?name=peter&back=/home`
To retrieve user information, the server queries an internal API with the following request: `GET /users/search?name=peter&publicProfile=true`

### Truncating query strings

We can use a URL-encoded `#` character to attempt to truncate the server-side request. To help us interpret the response, we could also add a string after the `#` character.
For example: `GET /userSearch?name=peter%23foo&back=/home`
The front-end will try to access the following URL: `GET /users/search?name=peter#foo&publicProfile=true`

> [!NOTE]
> 
> It's essential that we URL-encode the `#` character. Otherwise the front-end application will interpret it as a fragment identifier and it won't be passed to the internal API.

Review the response for clues about whether the query has been truncated. For example, if the response returns the user `peter`, the server-side query may have been truncated. If an `Invalid name` error message is returned, the application may have treated `foo` as part of the username. This suggests that the server-side request may not have been truncated. If we're able to truncate the server-side request, this removes the requirement for the `publicProfile` field to be set to true. We may be able to exploit this to return non-public user profiles.

### Injecting invalid parameters

We can use an URL-encoded `&` character to attempt to add a second parameter to the server-side request.
For example: `GET /userSearch?name=peter%26foo=xyz&back=/home`
This results in the following server-side request to the internal API: `GET /users/search?name=peter&foo=xyz&publicProfile=true`

If the response is unchanged this may indicate that the parameter was successfully injected but ignored by the application.

### Injecting valid parameters

For example, if we've identified the `email` parameter, you could add it to the query string as follows: `GET /userSearch?name=peter%26email=foo&back=/home`
This results in the following server-side request to the internal API: `GET /users/search?name=peter&email=foo&publicProfile=true`

Review the response for clues about how the additional parameter is parsed.

### Overriding existing parameters

We could try to override the original parameter.  For example: `GET /userSearch?name=peter%26name=carlos&back=/home`
This results in the following server-side request to the internal API: `GET /users/search?name=peter&name=carlos&publicProfile=true`

The internal API interprets two `name` parameters. The impact of this depends on how the application processes the second parameter. This varies across different web technologies. For example:
- PHP parses the last parameter only. This would result in a user search for `carlos`.
- ASP.NET combines both parameters. This would result in a user search for `peter,carlos`, which might result in an `Invalid username` error message.
- Node.js / express parses the first parameter only. This would result in a user search for `peter`, giving an unchanged result.

If we're able to override the original parameter, we may be able to conduct an exploit. For example, we could add `name=administrator` to the request. This may enable us to log in as the administrator user.

## Testing for server-side parameter pollution in REST paths

A RESTful API may place parameter names and values in the URL path, rather than the query string. For example: `/api/users/123`
- `/api` is the root API endpoint.
- `/users` represents a resource, in this case `users`.
- `/123`represents a parameter, here an identifier for the specific user.

Consider an application that enables us to edit user profiles based on their username. Requests are sent to the following endpoint: `GET /edit_profile.php?name=peter`
This results in the following server-side request: `GET /api/private/users/peter`

We may be able to manipulate server-side URL path parameters to exploit the API using [path traversal](https://portswigger.net/web-security/file-path-traversal) sequences. We could submit URL-encoded `peter/../admin` as the value of the `name` parameter: `GET /edit_profile.php?name=peter%2f..%2fadmin`
This may result in the following server-side request: `GET /api/private/users/peter/../admin`
If the server-side client or back-end API normalize this path, it may be resolved to `/api/private/users/admin`.

## Testing for server-side parameter pollution in structured data formats

Inject unexpected structured data into user inputs and see how the server responds.

Consider an application that enables users to edit their profile, then applies their changes with a request to a server-side API. When editing our name, the browser makes the following request:

`POST /myaccount 
name=peter`

This results in the following server-side request:

`PATCH /users/7312/update 
{"name":"peter"}`

 We can attempt to add the `access_level` parameter to the request as follows:

`POST /myaccount 
name=peter","access_level":"administrator`

If the user input is added to the server-side JSON data without adequate validation or sanitization, this results in the following server-side request:

`PATCH /users/7312/update 
{name="peter","access_level":"administrator"}`

This may result in the user `peter` being given administrator access.

---

Consider a similar example, but where the client-side user input is in JSON data. When editing our name, the browser makes the following request:

`POST /myaccount 
{"name": "peter"}`

This results in the following server-side request:

`PATCH /users/7312/update 
{"name":"peter"}`

We can attempt to add the `access_level` parameter to the request as follows:

`POST /myaccount 
{"name": "peter\\",\\"access_level\\":\\"administrator"}`

If the user input is decoded, then added to the server-side JSON data without adequate encoding, this results in the following server-side request:

`PATCH /users/7312/update 
{"name":"peter","access_level":"administrator"}`

Again, this may result in the user `peter` being given administrator access.


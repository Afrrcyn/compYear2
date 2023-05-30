It's an architecture style that can be implemented in many different ways. REST is the underlying architectural principe of the WWW.

The browser can operate without knowing anything about:
- The server
- The resources it hosts.

The key constraint of REST is that the server and client must both agree on the media used - for the web it's HTML.

Resources are manipulated with the HTTP verbs:
```http
POST   - Create
GET    -  Read
PUT    - Update
DELETE - Delete
```

We can rely on the fact that some verbs that are idempotent - so performing the operation multiple times has the same result as performing it the first time.
GET, PUT and DELETE are idempotent.

REST provides results that are self-descriptive and link to other resources as necessary
```json
{
	name : "Affrcyn",
	age  : 17,
	status: "single"
}
```
^ self-descriptive and makes sense from the HTTP verb we called (likely GET)

And remember that the server has no stored state for us, so we have to provide enough information in our calls:
```javascript
wait(GET, "/user/id/45678765")
```
or 
```javascript
wait(POST, "/user/new/", {...params})
```
We have to provide all the information as the server does not keep track of what we are doing.

##### Features of RESTful systems
Resources are identified by URIs (Uniform Resource Identifiers) and in the examples above I had the follow URIs
```javascript
"/user/id/45678.."
"/user/new/"
```

>[!note] URLs identify web address, URIs identify resources

The representation retrieved is dependent on the request and not the identifier. We can use accept headers to control whether you want HTML, XML or JSON.

```javascript
wait(GET, "/user/id/45678765", {accept : application/json})
```

We can use HTTP Cache-Control headers to determine how often we *actually* refresh the page, for instance Facebook News, we won't want to refresh every minute but maybe every 30 mins or something. This helps us to control the traffic a bit more.

goto: [[REST reading]], [[SWE 2 Home Page]]
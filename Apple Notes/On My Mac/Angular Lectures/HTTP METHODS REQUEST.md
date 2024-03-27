# HTTP METHODS REQUEST
#HttpBackend 

When we want to send HTTP Request we need to import **HttpClient** which also import on app.module.ts **(HttpClientModule)**. **it will handle send HTTP request to server** 
it have all HTTP methods and more you can used**.**
But HTTPClient methods are **Observable, so it needs to be subscribe before it can send the request to prevent a non-interested request**
 **(error reading attachment)**
**POST Request**
Expected 2 arguments
- First -> URL or API endpoint we want to send a request
- Second -> Data we want to send

 **(error reading attachment)**
**GET Request**
Expected 1 argument
- First -> URL or API endpoint we want to send a request

 **(error reading attachment)**
**DELETE Request**
Expected 1 argument
- First -> URL or API endpoint we want to send a request

**Using pipe to manage our data request**
 **(error reading attachment)**
 *using map pipe to fetch data from object to object array*

**Specific type data on HTTPClient**
 **(error reading attachment)**
you can see above that angular doesn’t detect our type value. 
because it just know what type of external data is ([]), but doesn’t know what inside data looks like (any) 
so you can specific it too, **for more understandable code and let angular accessing properties, method etc.** 
- **casting into generic type data (generic method)**
 **(error reading attachment)**
1. [**key: string]** -> is TS specify type syntax that means **every keys’s value in this object is ‘string’**
2. **Post** -> keys name (interface model)
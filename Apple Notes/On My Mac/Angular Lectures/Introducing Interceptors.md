Introducing Interceptors
#HttpBackend 

Interceptors allow us to intercept incoming or outgoing HTTP requests using the HttpClient. **By intercepting the HTTP request, we can modify or change the value of the request. Every time request had been triggered**
**for example authenticate user etc**

**Create Interceptors**
1. **create service for interceptors (auth-interceptors.service.ts)**
 **(error reading attachment)**
2. config on app.module.ts
 **(error reading attachment)**

![[Screen Recording 2565-05-11 at 16.10.17.mov]]

**Modified Request**
 **(error reading attachment)**
*with req.clone() you can now a accessing all responded methods as JS object*
*for example setting Http header and Http params for authenticate every time it get request*
 **(error reading attachment)**

 **Response Interceptors**
 **(error reading attachment)**
*because in the end it will return as observable so you can use these operator for manage response interceptors*
 **(error reading attachment)**

**When Using Multiply Interceptors, Order Is Matter!**
In case you have multiply interceptors and they are relative, order is matter

Ex. if you are ordered interceptors correctly
 **(error reading attachment)**
*send authenticate (http header) first before logged in*
 **(error reading attachment)**

if you aren’t order interceptors correctly
 **(error reading attachment)**
 **(error reading attachment)**
*there are no authentication headers, so it would break your app if you had to do something with header data*
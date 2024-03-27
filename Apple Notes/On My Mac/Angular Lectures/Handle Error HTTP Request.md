# Handle Error HTTP Request
#HttpBackend 

**Log error messages on user interface**
On subscribe, second arguments you can passed **error** as arrow function which error is a object
 **(error reading attachment)**
*store error objects and displayed some error propeties (use spread params to speard each each properties)*
 **(error reading attachment)**
 **(error reading attachment)**

**Using Subject for Error handling**
Used in case when HttpClient subscribe on service ts
1. Created Subject for passing error object
 **(error reading attachment)**
2. call Subject’s subscribe on TS component (don’t forget to destroy it)
 **(error reading attachment)**

**CatchError Operator and ThrowError Observable**
 **(error reading attachment)**
Use **catchError Operator** when we want to send error to analytics then use **throwError** observable to stop executing
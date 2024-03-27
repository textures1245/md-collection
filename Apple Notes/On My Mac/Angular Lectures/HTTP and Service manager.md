# HTTP and Service manager
#HttpBackend 

When we working on HTTPClient, sometime it would mess your code up if you’re coding on TS component.
So it should be nice if we can handle all HTTP requests on Service manager

there are 2 ways for using benefit from service manager
1. **When not interest in responded data**
In case we are not gonna do anything about responded data (Ex. when using POST, DELETE and don’t want to do anything about it)
**we can just call subscribe the observable on service** 
 **(error reading attachment)**
1. **When we are interest in responded data**
In case we want to manage our responded data (Ex. using GET to fetched modified data)
- **On service don’t call subscribe but return http instead**
 **(error reading attachment)**
- **In TS Component we had called service, you can just subscribe here**
 **(error reading attachment)**
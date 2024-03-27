# Angular route strategy
#route 

https://www.tektutorialshub.com/angular/angular-location-strategies/
 **(error reading attachment)**

**When working on real server (deployment server)**
When app send the url. first it send to the server (deployment server) then send to the framework (angular) to handled it 
But on angular concept is **one single page application. Is mean that**
- If path url that send to server, server will recognized that **if path that had been sent is correct or not (is this path has it own HTML or not) if not it will call a fallback (component we created when something wrong on navigation like 404 page etc. )**
- In angular that we told early that it an one single page application, 
	- **So when server is looking for html to rendered. it will not found that** 
	- **because Angular will all handle to us for everything (path, programatically etc)** 
	- **but it doesn’t send to angular yet. So it will call a fallback right away before it send to angular** 
	- **So it will look for ‘404 page’ (fallback path) every time we request a url**

**To solve this problem we can set option to hashMode on RouterModule**
 **(error reading attachment)**
*On Appmodule*

- *When enter to HashLocationStrategy*
- *When server retreated a url* ***it will ignored all the thing that in front of #*** 
- *It’s mean that it will not looking for children path it will look only at* ***the core path (before # -> ‘localhost4200’)***
- ***So it will not handle the part that angular would do for us, and it means that it won’t broke the path of course***


***So it something wrong happened, try to use this method***
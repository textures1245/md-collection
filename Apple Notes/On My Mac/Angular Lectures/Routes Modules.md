# Routes Modules
#route

Router modules can handle routes navigate on angular which can be setting on **app.module or router.ts**

**On app.module**
import { RouterModule, Routes }
 **(error reading attachment)**
 **(error reading attachment)**

**On AppRoutingModule**
1. Created app.routing.module.ts
2. Config below this
 **(error reading attachment)**
3. **Import AppRoutingModule in AppModule**
 **(error reading attachment)**

**Navigation**
 **(error reading attachment)**
- **RouterLink is a route navigation to navigate that defined on route configuration**
	- **Which have 2 patterns following by these**
		- **/routes -> absolute path (use when want to defined a absolute path)**
		- **routes -> relative path  (use when you want to nest routing path on each component)**
- **routerLinkActive -> using with css class for active button** 
- **routerLInkActiveOption -> config for active (exact -> only active when absolute path like a defined path )** 

 **Navigate on back-end (ts)**
 **(error reading attachment)**
 **(error reading attachment)**

 **Navigate relative path on back-end (ts)**
 **(error reading attachment)**
- ActivatedRoute module can tracks what current path we’re on. So we can use this for relative path, which used **relativeTo** option

**Snapshot/takes params from url/path** 
 **(error reading attachment)**
 **(error reading attachment)**
	use **snapshot.params[params from path configuration] from ActivatedRoute** to take params from specific params url
	-In this case take params and update on **id and name and display on user CPN** 
1. This will trigger when change directly on url or got lead url from other CPN **it will not works when get lead in CPN itself (this is a default angular behavior that not re-rendered when lead on same CPN)**
2. This method will solve problems above. it will always update every time the route had changes
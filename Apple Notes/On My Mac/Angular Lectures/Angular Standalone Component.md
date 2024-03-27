# Angular Standalone Component
#standalone


<p style="text-align:center;margin:0">## <b>What are "Standalone Components"?</b>
Standalone components are regular Angular components with one important twist: They don't require a @NgModule as a wrapper.
Instead, you can use them, well, "standalone". They define their own dependencies and can be imported and used anywhere.
</p>


![[bcnfe7K.jpeg]]

 
In v14 and higher, standalone components provide a **simplified way to build Angular applications**. Standalone components, directives, and pipes aim to streamline the authoring experience **by reducing the need for** <a href="https://angular.io/api/core/NgModule" rel="noopener" class="external-link" target="_blank" style="color:#e4afaff;"><b>NgModule</b></a>**s**. Existing applications can optionally and incrementally adopt the new standalone style without any breaking changes.


- **Building Standalone Component**
1. generate component || pipe || directive
2. on @Component decorator have **a standalone property which set it to true (false by default)**
 **(error reading attachment)**
3. **now you can get rid of declaration from NgModule (since we used standalone instead and it would get error if you still kept them on NgModule)**
4. there are 3 ways for making **a standalone works on other components**
	1. import them in NgModule’s imports on app.module (global)
 **(error reading attachment)**
	2. make other components works on standalone component, by import them on standalone component
 **(error reading attachment)**
*not only for component, you can import module too*

- &nbsp;
	1. *import standalone to other standalone component (only works if both are standalone CPN)*
 **(error reading attachment)**
*HighlightDirective is standalone too*

- **Fully Migrate app root CPN to standalone CPN**
*make app CPN to works as standalone without app.module (only works in case all children CPN are standalone)*
1. *make app CPN as standalone, then import others children CPN that are relative to app CPN (all children CPN need to be as standalone CPN too)*
 **(error reading attachment)**
2. to make it works, **we need to change configuration to start on app CPN instead of app module** 
3. on **main.ts** file remove these code lines,
 **(error reading attachment)**
and add these code lines instead
 **(error reading attachment)**
1. now we can remove app.module.ts file cause we don’t need it anymore (we used app CPN as root instead)

- **Standalone Service** 
- there are 3 ways for providing standalone service
1. add Injectable({provideIn: ‘root’}) on service (**global)**
2. provide services on **main.ts (global)**
 **(error reading attachment)**
3. provide services on standalone CPN **(local) **if you provide like this, service that on standalone CPN won’t shared state on other services that provide as global or local on other standalone CPN****
 **(error reading attachment)**

- **Standalone Routing**
- **1. import RouterModule on app CPN**
- **2. on main.ts file add this code line**
 **(error reading attachment)**

- **Standalone Routing with Lazy loader**
*when working with standalone CPN* ***you can used lazy loader dierectly or manage lazy loader easiser then normal CPN*** 
1. ***used lazy loader dierectly on standalone CPN (only works for standalone CPN)***
 **(error reading attachment)**
2. manage routes then overloaded them on routing
	1. manage routes (routes.ts)
 **(error reading attachment)**
1. overloaded them on routing
 **(error reading attachment)**

RoundUp: https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/32463346#content
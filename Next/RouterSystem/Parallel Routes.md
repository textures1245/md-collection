Allow to simultaneously or conditionally render multiple pages

**Syntax** : using under path app prefix with `@`  
- route-app
	- @parallel-routes

**Affection**
- Parallel routes will not effected to each other route when URL changes only navigated route will changed.
- It will keep browser's URL history state and active UI state since is correctly synced.

**Usage**
- Using for advanced routing patterns like Conditional Routes and Intercepted Routes 
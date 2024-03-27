# Lazy Loading & Preloaded
#appModule 

 **(error reading attachment)**
if we separate our modules to more specific. (EX. product.module, app.module, add.module) now **you can now enhance code performance by use technic called ‘Lazy Loading’**

**Concept is ‘Loading only module that belong to that path we currently visited’**
**Ex. if we are on ‘/admin’ only these module will be loaded**
- **AppModule**
- **AdminModule** 
And other won’t load if we are not visit yet. 
**This will load less code, and make our app faster than loaded all module that we may be not need it.**

**How to used** 
***(This approach must have separate specific modules for loading only useable component)***
1. **On specific routing module, on parent path start with empty string**
 **(error reading attachment)**
2. On **app.routing.module add new path for loading specific modules**
 **(error reading attachment)**
- take loadChildren property, then use arrow fuction to acessing module by import.
- this wil treat as promise so we can called then to take module
1. When we use this technic don’t forget to remove separate module on app.module to prevent overloading and restarted app.

**(path: /auth)**
 **(error reading attachment)**
***before use lazy loading*** 

 **(error reading attachment)**
***after use lazy loading (only load authModule)***

## **LazyLoading with Preloading**
somehow there is a case that we are on current path and may want to loaded other modules for prepareing when we want to navigated to other path, you can do that by config RouterModule on app.routing.module
 **(error reading attachment)**
this will be loaded **initialize module first, then when it done it will loaded other sequential modules next**
**these will giving more performance when working with ‘Lazy Loading’**
# AppModule
#appModule

 **(error reading attachment)**
For the bigger project, it may be confusion if there are a lot of components that only kept on **app.module.** when working with big project and team. It would be nice if we had organized our **angular’s** **module (ngModel)** for more easier to manage our flow work and enhance performance.

1. **Split to part of modules**
*A module that we splitted from app module (recipe.module.ts)*
 **(error reading attachment)**

1.1 these are component that we grouped for only **recipe components**
1.2 these modules need to be import because it related to our code (although we split module, **in the end** **children modules need to be export to app.module and ngModel itself it work “standalone” so it need work with angular’s modules that we import from app.module as well**)
1.3 when we declared to let angular knows our component we need to exports as well then import this module to app.module.ts 
(side note: if you embedded these component from other module {EX. loading from recipe.routing.module } you don’t need to export them ) 
 **(error reading attachment)**
*app.module.ts*

1. *Split routers*
router that we splitted from app.routing.module (recipe.routing.module)
 **(error reading attachment)**
when split our routing make sure that **children route modules, on import it should be RouterModule.forChild() to let angular connected to our main route (app.routing.module)**
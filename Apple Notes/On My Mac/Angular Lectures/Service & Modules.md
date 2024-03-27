# Service & Modules
#appModule 
 **(error reading attachment)**

**Beware of provide services on modules**
Module have a lot of kind you can work with (above for example) when you provide service **you must know what kind of module you are working on**
- Every kind of module have thier scope of **providing service (first row)** 
- So you need to know **what kind of module injection is (second row)** 
**if you are provided service on a module that may be the wrong purpose you expected to. It may cause a unexpected behavior and didn’t alert any errors becasue it not a bug**

**more detail: https://www.udemy.com/course/the-complete-guide-to-angular-2/learn/lecture/14466516#content**

**1. What is Eager Loading?**
Feature modules under Eager Loading would be loaded before the application starts. This is the default module-loading strategy.
**2. What is Lazy Loading?**
Feature modules under Lazy Loading would be loaded on demand after the application starts. It helps to start application faster.

**When to use Eager Loading?**
**Case 1**: Small size applications. In this case, it’s not expensive to load all modules before the application starts, and the application will be faster and more responsive to process requests.
**Case 2**: Core modules and feature modules that are required to start the application. These modules could contain components of the initial page, interceptors (for authentication, authorization, and error handling, etc.), error response components, top-level routing, and localization, etc. We just have to eagerly load these modules to make the application function properly despite the application size.

**When to use Lazy Loading?**
The scenario of applying Lazy Loading is relatively simple and straightforward. In a big-size web application, we can lazily load all other modules that are not required when the application starts.
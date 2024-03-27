Operators and Subject
#observable

**Operators**
In RxJS, they have **operators which can be import and used with observer.**
1. **to start, we need to add “.pipe” on observable before we called subscribe**
2. **then we can use operators by using with arrow functions** 
 **(error reading attachment)**
***filter ->*** *will filter outcomes which determined on how condition is (log data on console when data is greater than 0)*
***map ->*** *will map outcomes before it get called from subscribe (Round …)*
 **(error reading attachment)**
*example when using map pipe to fetch data from object to object array*

**Subject**
 **(error reading attachment)**
**Subject is like EventEmitter but it’s observable and better then EventEmitter if we want to handle something that can be triggered from code** 

***for example when we click event on some component and then triggered to other component***
1. ***create service to injected data across components***
 **(error reading attachment)**
2. created eventListener to be triggered on app component
 **(error reading attachment)**
then call next (on EventEmitter called emit) and passing data we want to subscrible  
 **(error reading attachment)**
1. call subject observable to subscribe data and destory it
 **(error reading attachment)**
 **(error reading attachment)**
 **(error reading attachment)**
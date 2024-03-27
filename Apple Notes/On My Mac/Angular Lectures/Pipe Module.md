# Pipe Module
#pipe
 **(error reading attachment)**
**Pipe** allow you to transform output in the template which doesn’t changed the value itself from component, which pipe can work for both synchronous and asynchronous

Which pipe allow you to have arguments in case when we want to use as dynamic pipe which be can use by add simicolon **:** in front of pipe follows wtih argument
 **(error reading attachment)**
you can aslo chainned pipes too, **but remember it will execute(casting) from left to right**
means that order is matter like for this example if we ordered uppcase first
 **(error reading attachment)**
***this will not work, cuz is can’t casting from string to date type*** 
 **(error reading attachment)**

**Created Own Custom Pipe**
1. create filename.pipe.ts (you can generate by type -> ng g p nameFile)
2. add @Pipe decorator which has ‘name’ property to export pipe for using on Template
3. export class and implement ‘PipeTransform’
4. must have ’transform’ method
	1. on params you can have 2 more params, which first is **value** (value that we want to use on pipe) and second is **arguments** (pipe’s arguments) which can have more then 1
 **(error reading attachment)**
1. Import this pipe in declaration on app.module.ts

**Understand how pipe’s executing works**
When using pipe if in case when we want to output some dynamic data (always re-render/update output)
By default it will not show re-render output automatically. for this example

![[Screen Recording 2565-05-09 at 13.48.38.mov]]

1. *we created new server, it will update automatically when we not using our pipe (the search bar is a custom pipe we created, it will matched if input is the same as serverStatus)*
2. *but when we using our pipe and add new server you can see it not update our new servers but it will show again if we don’t use search bar or reseach again*
3. ***This is a default pipe’s behavior. It will not re-render output automatically but it will kept it and displayed when we executed it later for keep perfomance excuting which it will not slow down our app***
4. *but you can set it to displayed output automatically. On @Pipe decorator in properties you can add* ***Pure: false (true is default)***
5. *which will allow to always kept update output, but again* ***it will might slows down our app*** *but use in case if we know what we’re doing*


***Async Pipe***
*This pipe allow you to output asynconous output (callback, promise, observable)*
 **(error reading attachment)**
 **(error reading attachment)**
# Creating own observable
#observable 

Like we had known, Observable is a third-party object packaged that we can use to handle **asynchronous task** like **RxJS package**

**To understand how observable works, look at this example.**
1. **Import RxJS Package**
2. Easiest way to creating our own observable is use **interval from RxJS**
 **(error reading attachment)**
***Must remember. When we created our own observable, we need to unsubscribe and destroy it when we’re not using it anymore.***
***To not leak out any memory, and slow down our app.***

1. *We can do destroy it by* ***create variable to kept observable then unsubscribe and destroy them on ngOnDestory lifecycle hook***
 **(error reading attachment)**
 **(error reading attachment)**

**Creating Own Custom Observable**
 **(error reading attachment)**
1. create **new Observable**
2. on **Observer argument** is the thing that is interest in begin informed about new data, errors or completed **which is a part of listener**
3. on setInterval in **observer.next(count)** is method that tell observer what we are gonna do next which have 3 methods
	1. next -> tell observer to emit that argument
	2. error -> tell observer to throw error
	3. complete -> tell observer when we are done our task (argument we passed on)
1. call a Observable that we had been created
 **(error reading attachment)**
2. then unsubscribe and destroyed it 
 **(error reading attachment)**
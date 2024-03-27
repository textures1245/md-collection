# Adding navigation with event binding and ngIf
#event

1. On header html component **we have nav bar to navigate to order components**
 **(error reading attachment)**
	So basically, we created events to bound them to parent component (app-component) 
 **(error reading attachment)**
	which in argument is data to send what component did we visit to
2. On header TS, We create a custom event (EventEmitter) and method to emit event data to parent component
 **(error reading attachment)**
	2.1 **onSelecte method** will get data from frontend (when user clicked on nav bar) and emit to featureSelected
	2.2 **featureSelected** is custom event that we used to passing event data to parent component (use @Output decorator to create en 	event emitter and export it to parent component)

1. Back to app component, we called custom event **featureSelected** with event binding to get data and
 **(error reading attachment)**
	3.1 call new method name **onNavigate($event)** to handle showing template, which **$event is event data that we emitted from 	header CMP and passing thought featureSelected.**
 **(error reading attachment)**
	3.2 Back to app TS, we created new var name **loadedFeature** to stored event data that we got from header CMP
	3.3 created method onNavigate(feature: string) to load feature from event data every time is featureSelected has emitted
	3.4 use **loadedFeature to check what current template component we on now**
 **(error reading attachment)**

 **(error reading attachment)**
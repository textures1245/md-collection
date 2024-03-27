# Dynamic Component
#dynamicComponent

 **(error reading attachment)**
A Component that will be showed/loaded which is set by programmatically
before make it works, we need to prepared what we need to do dynamic components
- **Template** 
 **(error reading attachment)**
- **Component**
 **(error reading attachment)**

1. **ngIf**
 **(error reading attachment)**
just sending data that we want to output to dynamic component


1. **Dynamic component loader**
this method can created a dynamic component by programatically (TS) and display on ng-template which giving more powerful for handling dynamic component, **but this method is deprecated. it shouldn’t be used because angular it not supported it anymore**

**— What we need to prepared for this method**
- **Directive (for a template reference)**
 **(error reading attachment)**
- created ng-template with directive maker (appPlaceHolder)
 **(error reading attachment)**
- Used @ViewChild to ref element
 **(error reading attachment)**
- Created method to triggered a dynamic component
 **(error reading attachment)**

- Created component which ref on component we want to display (AlertComponent)
- Connect viewContainerRef of directive we created
- Clear old component before started created new component

2.
- for **instance** method you can reference variable of **Component Factor (alertCmpFactor)**

 **(error reading attachment)**
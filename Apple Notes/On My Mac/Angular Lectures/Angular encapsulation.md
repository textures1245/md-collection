# Angular encapsulation
#knowlaged
## **CSS**
	For angular css stylesheet component, Each component has it own css stylesheet which by default it couldn’t be relative with other components
 **(error reading attachment)**
	- On this p element which has attribute name “ngcontent-tns-c42”
	- It’s a angular css name generated. Which refer to **their own CSS encapsulation**
	- and it will generated a different name for each component

You can setting this by import a ViewEncapsulation on @angular/core
 **(error reading attachment)**
	-None -> it will shows css component. when on set, this CSS component will not longer private
	-ShadowDom -> a DOM technology, it will hidden css component like angular does, but it’s not supported for every browsers
	-Emulated -> default setting
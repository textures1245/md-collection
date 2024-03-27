1. Svelte had `svlete\store` which have ablities to created **State managment** which can be apart by
	1. `writable` : a function to craeted *writable state* when we created it return 2 following methods
		1. `set` -> it can be set *new value* to state
		2. `update` -> it can be updated value by *modified old value* 
		the diffrence between them are `set` will set the new value while `update` just modify the value
	2. `readable` : a function to created *readonly state* when we created it
		1.  `set (callback)` -> method to set value on callback function

the only diffrence between is `writable` can be updated but `readable` can't
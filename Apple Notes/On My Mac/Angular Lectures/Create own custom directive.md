# Create own custom directive
#Directive
1. Create a custom directive file which name it **nameFile.directive.ts**
2. Then import **Render2 and ElementRef to refer a DOM that want to be used**
 **(error reading attachment)**
3. In @Directive in selector should name it with camelCase style and add [] to it
 4. Declared variables to used modules
 **(error reading attachment)**
5. Used ngOnInit lifecycle hook to used cusotom directive behavior 
6. Call directive
 **(error reading attachment)**

**Another the easier way approach is using @HostBinding decorator**
1. Import HostBinding
2. Create @HostBinding decorator
 **(error reading attachment)**

**! Created own custom directive you shouldn’t change DOM directly, but you should passing it with Render2.**
**When change DOM in directive way, it may cause error**

**Bad Approach for creating custom directive**
 **(error reading attachment)**
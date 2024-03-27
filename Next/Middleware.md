- `Next.js` middleware is based on the [Web Fetch API](https://nextjs.org/docs/pages/building-your-application/routing/middleware), which is a standard interface for fetching resources from the network. The middleware function takes a [NextRequest](https://blog.logrocket.com/using-next-js-middleware-edge-functions/) object as an argument and returns a [NextResponse](https://javascript.plainenglish.io/next-js-middleware-how-it-works-and-5-real-use-cases-cfacbeb810c9) object or a [Promise] that resolves to a NextResponse object.
  
  **Create**
```TS
// middleware.ts  
import { NextResponse } from 'next/server'  
import type { NextRequest } from 'next/server'  
export function middleware(request: NextRequest) {  
// Your middleware logic here  
return NextResponse.next()  
```

middleware function will be invoked for every route in your project, unless you specify a custom matcher config.

```TS
// middleware.ts  
export const config = {  
matcher: '/about/:path*',  
}
```

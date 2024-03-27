# HTTP header, query params and respond type/body
#HttpBackend 

**Setting HTTP Headers**
HTTP headers let the client and the server pass **additional information with an HTTP request or response**. An HTTP header consists of its case-insensitive name followed by a colon (:), then by its value. <a href="https://developer.mozilla.org/en-US/docs/Glossary/Whitespace" rel="noopener" class="external-link" target="_blank" style="color:#e4afaff;"><u>Whitespace</u></a> before the value is ignored.

Headers can be grouped according to their contexts:
- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Request_header" rel="noopener" class="external-link" target="_blank"><u>Request headers</u></a> contain more information about the resource to be fetched, or about the client requesting the resource.
- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Response_header" rel="noopener" class="external-link" target="_blank"><u>Response headers</u></a> hold additional information about the response, like its location or about the server providing it.
- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Representation_header" rel="noopener" class="external-link" target="_blank"><u>Representation headers</u></a> contain information about the body of the resource, like its <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types" rel="noopener" class="external-link" target="_blank"><u>MIME type</u></a>, or encoding/compression applied.
- <a href="https://developer.mozilla.org/en-US/docs/Glossary/Payload_header" rel="noopener" class="external-link" target="_blank"><u>Payload headers</u></a> contain representation-independent information about payload data, including content length and the encoding used for transport.
*ref:* [*https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers*](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

**Every HttpClient’s method has a extra last argument for passing a option object for config a request**

**Setting HTTP Headers**
when want to set a HTTP Header you need to import **HttpHeader** for config a HTTP header
 **(error reading attachment)**
 **(error reading attachment)**

**Query params**
1. **Single query params**
 **(error reading attachment)**
 **(error reading attachment)**
2. **Multiply query params**
 **(error reading attachment)**
*append how many you want for query params*
 **(error reading attachment)**

**Observing Different Types of Responses**
Use in case we want to accessing more response/request data to debugging, accessing more features or prevent some errors etc;
there are couple ways you can use to accessing data
1. **return all responsed data**
 **(error reading attachment)**
 **(error reading attachment)**
2. **return only responses’s body -> observe: ‘body’**
 **(error reading attachment)**
3. **use HttpEventType for created condition**
**observe: ‘events’** will return event response, which each response has it own type and it mark as **number**
 **(error reading attachment)**
**you can use tap Operator for created some condition before it got subscribe**
 **(error reading attachment)**
 **(error reading attachment)**
1. console.log(event) -> show what event type is (0 means that it HttpEventType.Sent)
2. it responded back (after DELETE) so now event type is 4 (HttpEventType.Response = 4)
3. So it log event.body, but now response’body got deleted. So it shows ‘null’ instead

**Changing the Response Body Type**
Use in case you want to changing response body data type (default it would return JSON)
 **(error reading attachment)**
 **(error reading attachment)**
***this body’s value is now treating ‘string’ instead of Null***
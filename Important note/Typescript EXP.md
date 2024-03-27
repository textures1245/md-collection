**You can't**

- **Generic type**
  
> You can't make everything as dynamic type, so consider using `any` if you know what you doing.
```TS
let getData = async <T extends () => ReturnType<T>>(d: Promise<T>) => d;

let data: any[] = [];

if (dataType === 'LISTS') {

// data = await getData(getData) 
// CAUSE -> Argument of type '() => Promise<List[]>' is not assignable to parameter of type 'Promise<() => any>'.ts(2345)

// Instead just make varaible as any to handle dynamic ReturnType but also make sure to handle for manuipulated this variable 
data = await getList();

}

const keys = Object.keys(filterType[dataType]) as Array<

keyof typeof filterType[typeof dataType]

>; // An array of keys


let actualFields = data.map((d) => {

let obj = {} as typeof filterType[typeof dataType];

keys.forEach((key) => {

obj[key] =

d.data[key] instanceof Timestamp

? (d.data[key] as Timestamp).toDate().toLocaleDateString()

: String(d.data[key]);

});

return obj;

});
```


```TS
async function exportDataAsXLSX<T extends string[]>(
		fields: T,
		dataType: Exclude<keyof typeof filterType, 'USER'>
	) {
		// let actualFields =
		// 	fields.length === 0 ? getKeyAsArray(filterType[valueBind.dataType!]) : fields;

		let getData = async <T extends () => ReturnType<T>>(d: Promise<T>) => d;
		
		if (dataType === 'LISTS') {
			// data = await getData(getData) 
			// CAUSE -> Argument of type '() => Promise<List[]>' is not assignable to parameter of type 'Promise<() => any>'.ts(2345)

			// Instead just make varaible as any to handle dynamic ReturnType but also make sure to handle for manuipulated this variable 
			data = await getList();
		}
		const keys = Object.keys(filterType[dataType]) as Array<
			keyof typeof filterType[typeof dataType]
		>; //  An array of keys

		// handle data manuipulated  
		let actualFields = data.map((d) => {
			let obj = {} as typeof filterType[typeof dataType];
			keys.forEach((key) => {
				obj[key] =
					d.data[key] instanceof Timestamp
						? (d.data[key] as Timestamp).toDate().toLocaleDateString()
						: String(d.data[key]);
			});
			return obj;
		});

		const params = {
			arr: actualFields,
			opts: valueBind.optsFields.length > 0 ? valueBind.optsFields : undefined
		};
```
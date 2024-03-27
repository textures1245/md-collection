This will include how you can basic setup.
1. Connection with Database Connection string (PostgresSQL hosting with Supabase)
	1. install `@prisma/client`
	2. `npx prisma init` to generate your prisma setup
	3. Setup your schema dababase
	 ![[Pasted image 20230623114840.png]]
	4. Creating a supabase project to host your db
	5. Get **Connection string** Database URL to link to your prisma tool
	6. Store the **Connection String** to `.env` file and replaced it will the following rules
		1. On the password if you had special character below this replace those special character to the new one https://stackoverflow.com/questions/63684133/prisma-cant-connect-to-postgresql
		2. on the port 
			- for development you need to change it to your localhost dev
			- for severless function like hosting your app with **Vercel** you need to change it to **Connection pool port** https://supabase.com/docs/guides/integrations/prisma#connection-pooling-with-supabase

### inside /app folder
- create a new `api` folder for the controllers
- in this folder create a new folder for every endpoint, inside the folder put the `route.ts` file

### route.ts
```ts
import { NextResponse, NextRequest } from 'next/server'

type ResponseData = {
	message: string,
}

export async function POST(req: NextRequest): Promise<NextResponse<ResponseData>> {
	try { 
		return NextResponse.json({ message: "ok"}, { status: 200 });
	} catch (error) {
		console.error("Error processing request:", error);
		return NextResponse.json({ message: "Internal Server Error"}, { status: 500 });
	}
}
```
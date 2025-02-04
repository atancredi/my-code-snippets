```python
from fastapi import FastAPI, Request, Response
import uvicorn

# Define app
app = FastAPI(
	title="",
	version=1.1,
	description="",
	redoc_url=None,
	openapi_url=None
)

# Health
@app.get("/health")
async def health():
	return {"message": app.title+" "+str(app.version)+" Alive"}

ws = uvicorn.Server(
	config = uvicorn.Config(
		app=app,
		port=8080,
		host="0.0.0.0",
		log_level="info",
		log_config={
			"version": 1,
			"disable_existing_loggers": False,
		}
	)
)

if __name__ == "__main__":
	print("Ready to start")
	ws.run()
```
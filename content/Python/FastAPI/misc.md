
## Mount static files
```python
from fastapi.staticfiles import StaticFiles
app.mount("/endpoint", StaticFiles(directory="path",html=True), name="path")
```

&nbsp;
## Settings from env
```python
from fastapi import FastAPI
from pydantic_settings import BaseSettings
class Settings(BaseSettings):
	app_name: str = "Awesome API"
	admin_email: str
	items_per_user: int = 50

settings = Settings()

app = FastAPI()
@app.get("/info")
async def info():
	return { "app_name": settings.app_name, "admin_email": settings.admin_email, "items_per_user": settings.items_per_user, }
```

```python
from fastapi.staticfiles import StaticFiles
from fastapi.responses import FileResponse

@app.get("/")
def index():
    return FileResponse("frontend/dist/index.html")

@app.exception_handler(404)
async def exception_404_handler(request, exc):
    return FileResponse("frontend/dist/index.html")

app.mount("/", StaticFiles(directory="frontend/dist/"), name="ui")
```
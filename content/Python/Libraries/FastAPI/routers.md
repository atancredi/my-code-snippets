### in the router file 
```python
from fastapi import APIRouter

router = APIRouter(
	prefix="/items",
	tags=["items"],
	responses={404: {"description": "Not found"}},
)

@router.get("/users/", tags=["users"])
async def read_users():
	return [{"username": "Rick"}, {"username": "Morty"}]
```

### in the main app file 
```python
from routers import router
app.include_router(router.router)
```
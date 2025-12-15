# Basic authentication
run `pip install python-dotenv`
```python
from fastapi import Depends, FastAPI, HTTPException, status  
from fastapi.security import HTTPBasic, HTTPBasicCredentials  
import secrets  
  
app = FastAPI()  
  
# Add a basic HTTP authentication  
security = HTTPBasic()  
  
def validate_credentials(credentials: HTTPBasicCredentials = Depends(security)):  
  
	# encode the credentials to compare  
	input_user_name = credentials.username.encode("utf-8")  
	input_password = credentials.password.encode("utf-8")  
  
		# DO NOT STORE passwords in plain text. Store them in secure location like vaults or database after encryption.  
	# This is just shown for educational purposes  
	stored_username = b'dinesh'  
	stored_password = b'dinesh'  
  
	is_username = secrets.compare_digest(input_user_name, stored_username)  
	is_password = secrets.compare_digest(input_password, stored_password)  
  
	if is_username and is_password:  
		return {"auth message": "authentication successful"}  
  
	raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED, detail="Invalid credentials", headers={"WWW-Authenticate": "Basic"})    
@app.get("/users/me")  
async def read_current_user(username: str = Depends(validate_credentials)):  
	return {"message": username}
```

# JWT Bearer auth
run `pip install python-dotenv pyjwt`

```python
from os import environ as env

from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from jose import jwt, JWTError
from dotenv import load_dotenv
load_dotenv()

API_SECRET = env.get("API_SECRET")
ALGORITHM = "HS256"
bearer_security = HTTPBearer()

def verify_jwt_token(
    credentials: HTTPAuthorizationCredentials = Depends(bearer_security),
):
    if not API_SECRET:
        # If SECRET is missing, skip authentication (like Go version)
        return None
    token = credentials.credentials
    try:
        payload = jwt.decode(token, API_SECRET, algorithms=[ALGORITHM])
        return payload
    except JWTError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid or expired token",
            headers={"WWW-Authenticate": "Bearer"},
        )
```
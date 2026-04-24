# jwt api


```python
# main.py
# [project]
# requires-python = ">=3.10"
# dependencies = [
#     "fastapi>=0.136.0",
#     "pyjwt>=2.12.1",
#     "uvicorn[standard]>=0.44.0",
# ]

import time
from fastapi import FastAPI
from fastapi.responses import PlainTextResponse
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
import jwt

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        # "https://<ngrok-tunnel>.ngrok-free.app",
        "http://localhost:5173",
        "http://127.0.0.1:5173",
    ],
    allow_credentials=True,
    allow_methods=["*"],  # Allows all HTTP methods (GET, POST, PUT, DELETE, etc.)
    allow_headers=["*"],  # Allows all headers
)


class TokenRequest(BaseModel):
    token: str = "secret"


@app.post("/verify-token", response_class=PlainTextResponse)
async def generate_jwt(request: TokenRequest):
    """
    Expects a JSON body with a 'token' parameter (secret key for the JWT)
    Returns a raw JWT
    """

    # Calculate expiration time: 30 minutes (1800 seconds) from now
    exp_timestamp = int(time.time()) + 1800

    # Payload with only the 'exp' claim to match the CLI exactly
    payload = {"exp": exp_timestamp}

    # Encode using HS256 and the secret "secret"
    encoded_token = jwt.encode(payload, request.token, algorithm="HS256")

    return encoded_token
```
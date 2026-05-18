# How to test locally
Google provides the Functions Framework exactly for this purpose. It spins up a local Flask server to simulate the Cloud Function environment.

- ### Step 1: Save your code and define dependencies
    Save your Python script as main.py. In the same directory, create a requirements.txt file and add the framework:
    ```
    functions-framework==3.5.0
    flask==3.0.0
    ```

- ### Step 2: Install dependencies
    Open your terminal and run:
    ```bash
    pip install -r requirements.txt
    ```

- ### Step 3: Start the local server
    Run the framework, pointing it to your function's name:
    ```bash
    functions-framework --target=defined_function --debug
    ```
    Note: The server will start, usually on http://localhost:8080.

- ### Step 4: Trigger the function
    Open a new terminal window and use curl to send a POST request:
    ```bash
    curl -X POST http://localhost:8080 \
    -H "Content-Type: application/json" \
    -d '{"param1": "value1}'
    ```

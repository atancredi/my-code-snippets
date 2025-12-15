GCF is a serverless execution environment for building and connecting cloud services.

The typical use cases of Cloud Functions are real-time data processing, webhooks, lightweight APIs, mobile backend and IoT, etc.

## Deployment
To deploy a Cloud Function, we need at least the following two files:

- `main.py` - a Python script containing the function for execution
- `requirements.txt` - a list of the required dependencies for the Python programme

# Caveats
- the function must return a value or a Promise response.
	in the case of Node apps, return 1; clearly tells the function to stop.
	Remeber, though, to set the status code of the request and end it, otherwise the function will stop without closing the response.
## Pricing
According to [GCP documentation](https://cloud.google.com/functions/pricing), the price of using Cloud Functions is dependent on

- how long your function runs
- how many times it’s invoked
- how many resources you provision for the function and
- whether the function makes an outbound network request
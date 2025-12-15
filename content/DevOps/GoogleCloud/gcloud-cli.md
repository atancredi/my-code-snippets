


# Credentials and Authentication
- Applications running locally “magically” start working after you install the Google Cloud SDK and execute `gcloud auth application default-login`. Applications then run using your user credentials.
- Applications running on GCP (a GCE instance, Cloud Run, …) authenticate themselves using the service account that you configure when setting up the service, _even if you are running the application on images where Cloud SDK is not installed_.
- Setting the `GOOGLE_APPLICATION_CREDENTIALS` environment variable authenticates you "manually" and overrides the above. Point the variable to a JSON credentials file on your local filesystem and bam, your application runs using the credentials in the file!

## switching between profiles
- `gcloud auth login` opens the browser and lets you login with your google account.
- `gcloud config set project <PROJECT_NAME>` switches the project

`gcloud auth application-default login`
- per verificare quale utente e' connesso a gcloud


`gcloud auth list` PER VEDERE CON CHE UTENTE SEI LOGGATO
`gcloud config list` PER VEDERE PROGETTO E REGIONE

### get credentials for project
`gcloud container clusters get-credentials <CLUSTER_NAME> --zone <ZONE> --project <PROJECT_NAME>`

---
# SSH
## SSH on a Cloud Platform VM
```bash
gcloud compute ssh --zone "<ZONE>" "<VM_INSTANCE_NAME>" --project "<PROJECT_ID>"
```


## Macchina virtuale
#### Avvia istanza della MV hal-eu
```
gcloud compute instances start hal-eu --zone=europe-west1-b --project=plg-chatbot
```
#### Avvia console SSH per MV hal-eu
```
gcloud compute ssh --zone "europe-west1-b" "hal-eu"  --project "plg-chatbot"
```


---
# Port forwarding

### Apre il tunnel con il servizio Kubernetes

```bash
kubectl port-forward service/<SERVICE> --namespace default 8080:80
```

---
# Deploy

## pre-deploy routine
- check the user
	`gcloud auth list`
- if wrong account set active acc
	`gcloud config set account <ACCOUNT>`
- check the project
	`gcloud config list`
- if wrong project set it
	`gcloud config set project <PROJECT_ID>`


---
# Issues
 - ## Deprecated Auth Plugin
	 For example when kubectl diff -k outputs "no openapi getter" or any other command within gcloud/kubectl outputs "deprecated auth plugin install ..."
	 - `export USE_GKE_GCLOUD_AUTH_PLUGIN=True`
	- `gcloud container clusters get-credentials <CLUSTER_ID> --zone <ZONE> --project <PROJECT_ID>`
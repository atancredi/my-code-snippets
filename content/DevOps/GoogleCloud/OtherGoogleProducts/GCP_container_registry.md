

# push an image to GCR
bisogna aver autenticato docker
`gcloud auth configure-docker europe-docker.pkg.dev`

---
`docker build -t telegram-miniapp:1235 .`
`docker image tag telegram-miniapp:1235 eu.gcr.io/plg-chatbot/telegram-miniapp:1235`
`docker image push eu.gcr.io/plg-chatbot/telegram-miniapp:1235`


### nuovo repo
`docker image tag virtualassistant-backoffice-envs-api:$TAG europe-docker.pkg.dev/plg-chatbot/eu.gcr.io/virtualassistant-backoffice-envs-api:$TAG`
`docker image push europe-docker.pkg.dev/plg-chatbot/eu.gcr.io/virtualassistant-backoffice-envs-api:$TAG`

after pushing a new version
kill revision to force kubernetes pod to pull latest revision from repository
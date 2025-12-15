

## Download an object from a bucket
```bash
gcloud storage cp gs://BUCKET_NAME/OBJECT_NAME SAVE_TO_LOCATION
```

## Download a folder of objects from a bucket
```bash
gcloud storage cp --recursive gs://BUCKET_NAME/FOLDER_NAME .
```

## List objects in a bucket
```bash
gcloud storage ls gs://nyg-tts --project PROJECT_NAME
```

## Upload a folder to the bucket
```bash
gcloud storage cp -r LOCAL_DIRECTORY_PATH gs://BUCKET_NAME/OBJECT_NAME
```


## Upload a file to the bucket
```bash
gcloud storage cp LOCAL_FILE_PATH gs://BUCKET_NAME/OBJECT_NAME
```

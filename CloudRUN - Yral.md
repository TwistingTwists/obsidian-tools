
#### Step 1 - Enable API 


You are about to enable:

- Artifact Registry API
- Cloud Build API
- Cloud Run Admin API
- Cloud Logging API

Enable


#### Step 2

```sh
```

```sh 
export PROJECT_ID=abhishek-tripathi-experiments
gcloud projects describe $PROJECT_ID
```

```sh
export PROJECT_ID=abhishek-tripathi-experiments
export PROJECT_NUMBER='460421532578'

gcloud projects add-iam-policy-binding abhishek-tripathi-experiments \    --member=serviceAccount:460421532578-compute@developer.gserviceaccount.com \    --role=roles/cloudbuild.builds.builder
```

### Step 3 



### Step 4
# Content insight Bot
Simple python application that fetches app metadata from appstore and google playstore

## Design
`sf-content-insight-bot` is a web service that listens on port `5000`  
`get_meta.py` is a python client that requests metadata from `sf-content-insight-bot`  

## Prerequisites
- Python 3.8
- Docker (https://docs.docker.com/install)
- Docker-compose (https://docs.docker.com/compose/install)

## How it works
`sf-content-insight-bot` runs as a Cloud Function on GCP  

## How to deploy as a Cloud Function
- Install Cloud SDK in local (https://cloud.google.com/sdk/docs)
- Update `gcloud` components
```sh
gcloud components update
```
- Create a service account (https://cloud.google.com/iam/docs/service-accounts)
In Google Cloud Console,  
Step 1: Select "IAM & admin" -> "Service accounts" from left side panel  
Step 2: Click "CREATE SERVICE ACCOUNT" and give DETAILS including Name and ID and Click "CREATE" button  
Step 3: Select "Project" -> "Owner" as the role and click "CONTINUE" button  
Step 4: Select "APIs & Services" from left side panel in Cloud console, and select "Credentials" -> "Creat credentials" button   
Step 5: Select "Service account key" from dropdown list and select "JSON" as a "Key type", click "Create" button  
Step 6: Automatically downloads the "credential" in JSON format  
- Export the credential path in your local env
```sh
export GOOGLE_APPLICATION_CREDENTIALS="path to the credential.json"
```
- Deploy Cloud Function
```sh
gcloud functions deploy sf-content-insight-bot --source https://source.developers.google.com/projects/feisty-current-253218/repos/sf-content-insight-bot/moveable-alises/master --entry-point insight_bot --trigger-http
```

## How to run in local
```sh
git clone https://source.developers.google.com/p/feisty-current-253218/r/sf-content-insight-bot
cd sf-content-insight-bot
docker-compose up --remove-orphans
```

On the other termial, you can execute `client/get_meta.py` with `platform` and `identifier` parameters  
For example:
```sh
cd client
python get_meta.py android com.hotelnight.android.prod
```

All responses will have the form
```json
{
	"platform": "itunes/android",
	"identifier": "appId for the respective store",
	"data": [
		{
			"source": "url of the app in the store",
			"name": "name of the app in the store",
			"publisher": "developer of the app",
			"category": "genre of the app in the store",
			"raw": "raw response data from the store",
		},
	]
}
```
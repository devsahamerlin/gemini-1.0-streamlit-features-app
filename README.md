###########
```sh
PROJECT_ID=$(gcloud config get-value project)
REGION=europe-west4
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"

gcloud services enable cloudbuild.googleapis.com cloudfunctions.googleapis.com run.googleapis.com logging.googleapis.com storage-component.googleapis.com aiplatform.googleapis.com

python3 -m venv gemini-streamlit
source gemini-streamlit/bin/activate

pip install -r requirements.txt

streamlit run app.py \
--browser.serverAddress=localhost \
--server.enableCORS=false \
--server.enableXsrfProtection=false \
--server.port 8080

echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"

PROJECT_ID=$(gcloud config get-value project)
REGION=europe-west4
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"

SERVICE_NAME='gemini-app-playground' # Name of your Cloud Run service.
AR_REPO='gemini-app-repo'            # Name of your repository in Artifact Registry that stores your application container image.
echo "SERVICE_NAME=${SERVICE_NAME}"
echo "AR_REPO=${AR_REPO}"

gcloud artifacts repositories create "$AR_REPO" --location="$REGION" --repository-format=Docker

gcloud auth configure-docker "$REGION-docker.pkg.dev"

gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT_ID/$AR_REPO/$SERVICE_NAME"

gcloud run deploy "$SERVICE_NAME" \
--port=8080 \
--image="$REGION-docker.pkg.dev/$PROJECT_ID/$AR_REPO/$SERVICE_NAME" \
--allow-unauthenticated \
--region=$REGION \
--platform=managed  \
--project=$PROJECT_ID \
--set-env-vars=PROJECT_ID=$PROJECT_ID,REGION=$REGION
```#   g e m i n i - 1 . 0 - s t r e a m l i t - f e a t u r e s - a p p  
 
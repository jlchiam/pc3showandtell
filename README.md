# pc3showandtell Docker Container

I don't really understand what am I doing. So I tried to re-do the practical.

This project will have the following parts:
- `Docker Hello` - Displays "Hello World!" in four different heading sizes.
- `Docker nodejs` - No changes at the moment, but I may update it later.
- `Google Quickstart-cloudsamples`

<hr />

# Google Quickstart-cloudsamples

- Followed [Google's quickstart guide] (https://cloud.google.com/build/docs/build-push-docker-image?msclkid=fa9b75a1a68011ec866f86293bccb184)
- Create project and enable API
- Build a Docker image
- Deploy the image
- Automate the code change by trigger events

<hr />

1. Created a Google Cloud Project called "pc3devopsproject2)
2. Enabled Cloud Build and Artifact Registry APIs (*add image here later, ![The San Juan Mountains are beautiful!](/assets/images/san-juan-mountains.jpg "San Juan Mountains"*))
3. install and initialize Google Cloud CLI. If already installed, run this to get latest version `gcloud components update`
   - to install, just use a docker image `docker pull gcr.io/google.com/cloudsdktool/google-cloud-cli:latest`
   - to check if you have installed, `docker run --rm gcr.io/google.com/cloudsdktool/google-cloud-cli:latest gcloud version`
   - then need to authenticate `docker run -ti --name gcloud-config gcr.io/google.com/cloudsdktool/google-cloud-cli gcloud auth login`, I think it is to link gcloud with docker
   - after authenticate successfully, the credentials are preserved in the volume of the `gcloud-config` container

4. Create sample codes to build in the container image
   ```
   mkdir quickstart-docker
   cd quickstart-docker
   ```

5. In the cloud shell editor, create a new file called `quickstart.sh` with the following:

   ```
   echo "Hello, world! The time is $(date)."
   ```
   
6. Create a file named `Dockerfile` with no file extension, with the following:

   ```
   FROM alpine
   COPY quickstart.sh /
   CMD ["/quickstart.sh"]
   ```

7. In the terminal, run the following command to make quickstart.sh executable `chmod +x quickstart.sh`\

8. Create a Docker repository in Artifact Registry called `quickstart-docker-repo` with location `us-central1` with the description 'Docker repository'

   ```
   gcloud artifacts repositories create quickstart-docker-repo --repository-format=docker \ --location=us-central1 --description="Docker repository"
   ```

9. Verify that the repository is created `gcloud artifacts repositories list` (*add image here*)
10. Build the image. There are two ways.
    a. Using Dockerfile
    b. Using Build Config File

    a. using Dockerfile and then submit the image to the *Artifact Registry*
         - To do this, get the Cloud project ID `gcloud config get-value project` (*add image*)
         - At the same dir where `quickstart.sh` and `dockerfile` is located, run this command substituting the project-id with your project-id in the previous step
   
      ```
      gcloud builds submit --tag us-central1-docker.pkg.dev/project-id/quickstart-docker-repo/quickstart-image:tag1
      ```

    - So mine is like this: (*image*)
      
    - I chose to use Dockerfile, so at the end of the built and push to Artifact Registry, I get this (*image*)

   b. using a build config file.
      - In Google Cloud Shell Editor, at the same directory as `quickstart.sh` and `Dockerfile`, create a *build config file* named `cloudbuild.yaml`
      - At build time, Cloud Build automatically replaces `$PROJECT_ID` with your project ID, so you don't need to worry about replacing it yourself.
      
         ```
         steps:
         - name: 'gcr.io/cloud-builders/docker'
           args: [ 'build', '-t', 'us-central1-docker.pkg.dev/$PROJECT_ID/quickstart-docker-repo/quickstart-image:tag1', '.' ]
         images:
         - 'us-central1-docker.pkg.dev/$PROJECT_ID/quickstart-docker-repo/quickstart-image:tag1'
         ```

      - At the terminal, start the build `gcloud builds submit --config cloudbuild.yaml`
      
11. Checking is done via Google Cloud Console -> Cloud Build Page.
 
12. Google Cloud requires Cloud Run Admin and IAM Service Account User permissions before it can deploy an image to Cloud Run. (G Cloud Run automates the managing of server resources, so DevOps team can focus on developing the app).

   - At terminal, set environment variables to store the project ID and project number
   
      ```
      PROJECT_ID=$(gcloud config list --format='value(core.project)')
      PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format='value(projectNumber)')
      ```

   - Grant Cloud Run Admin role to Cloud Build service account (command line)
   
      ```
      gcloud projects add-iam-policy-binding $PROJECT_ID \
       --member=serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
       --role=roles/run.admin
      ```
   
   - I think this is the end result that you see in Google Cloud Console
   (*image*)
   
   - Grant IAM Service Account User role to the Cloud Build service account for the Cloud Run runtime service account:
   
      ```
      gcloud iam service-accounts add-iam-policy-binding \
       $PROJECT_NUMBER-compute@developer.gserviceaccount.com \
       --member=serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
       --role=roles/iam.serviceAccountUser
      ```
   
13. Deploy a prebuilt image




 

# Setting up Jenkins pipeline with GCP

Install following plugins  
- GCloud SDK Plugin (this is only wrapper need to install GCLOUD SDK in Jenkins VM)
- Google Cloud Storage (optional)
- Google Compute Engine (optional)

Create Service account in target project
- create secret key

Create credential of type `Google Service Account from private key`


- Install Gcloud install GCLOUD SDK in Jenkins VM

Following sample pipeline to check 
```


pipeline {
    agent any
    stages {

        stage('List GCP Buckets') {

        steps {
            sh "gcloud --version"
            withCredentials([file(credentialsId: 'glogin-sa-key', variable: 'GCP_SA_USER')]) {
                sh("gcloud auth activate-service-account --key-file=${GCP_SA_USER}")
                sh(returnStdout:true, script: "gcloud config set project my-poc-dilip")
                sh("gsutil ls gs://my-poc-dilip")
            }
         }
        }//List GCP buckets

    }
}

```
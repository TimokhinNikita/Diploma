pipeline {
    agent { label 'gcp-node' }
    environment {
        GCLOUD_CREDENTIALS=credentials('gcloud-credentials')
        GCLOUD_REGION='asia-east1-b'
        GCLOUD_PROJECT_ID='temporal-bebop-413719'
        GCLOUD_CLUSTER_NAME='vote-app-dip-cluster'

    }
    stages {
        stage ('Clone') {
            steps {
                sh '''
                    rm -rf *
                    git clone https://github.com/vlonemok/voting-app-diploma
                '''
            }
        }
        stage ('Connect Agent to GCP') {
            steps {
                sh '''
                    gcloud auth activate-service-account --key-file=$GCLOUD_CREDENTIALS
                    gcloud container clusters get-credentials $GCLOUD_CLUSTER_NAME --region $GCLOUD_REGION --project $GCLOUD_PROJECT_ID
                '''
            }
        }
        stage ('Apply k8s-specifications') {
            steps {
                sh '''
                    pwd
                    cd voting-app-diploma
                    kubectl apply -f k8s-specifications/
                '''
            }
        }
        stage ('Download and apply ingress-nginx') {
            steps {
                sh '''
                    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.5/deploy/static/provider/cloud/deploy.yaml
                '''   
            }
        }
    }
}
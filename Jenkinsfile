
pipeline {
    agent any

    environment {
       
        GCP_PROJECT = 'project2-449918'  // Replace with your GCP project ID
        GCP_CREDENTIALS = credentials('gcp-key') // Jenkins credential ID for Google Cloud service account
        GIT_REPO_URL = 'https://github.com/maddurakesh/tftest.git' // Your Git repository URL
        GIT_BRANCH = 'main' // The branch you want to use
    }

    stages {
        stage('Checkout Git Repo') {
            steps {
                echo "Cloning Terraform repository from Git...test Trinadh"
                git url: GIT_REPO_URL, branch: GIT_BRANCH
            }
        }


        stage('Terraform Init') {
            steps {
                echo "Initializing Terraform...aaa"
                sh 'terraform init'
            }
        }

        stage('Terraform Plan..') {
            steps {
                echo "Running Terraform Plan..."
                sh 'terraform plan -out=tfplan'
            }
        }

        stage('Terraform Apply') {
            steps {
                echo "Applying Terraform Plan..."
                // Automatically apply the changes
                sh 'terraform apply -auto-approve tfplan'
            }
        }

        stage('Cleanup') {
            steps {
                echo "Cleaning up workspace..."
                // Remove sensitive files and clean up
                sh 'rm -f terraform.zip'
                sh 'rm -rf .terraform'
                sh 'rm -f tfplan'
                sh 'rm -f gcp-credentials.json'
            }
        }
    }

    post {
        success {
            echo 'Terraform applied successfully! Google Cloud Storage bucket created.'
        }
        failure {
            echo 'Terraform apply failed!'
        }
    }
}

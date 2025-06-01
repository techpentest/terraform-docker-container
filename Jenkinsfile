pipeline {
    agent any
    
    tools {
       dockerTool 'docker'
       terraform 'tf'
    }


    environment {
        GIT_REPO = 'https://github.com/techpentest/terraform-docker-container.git'
    }

    stages {
        stage('Install Docker (if missing)') {
            steps {
                sh '''
                if ! command -v docker > /dev/null; then
                    echo "Docker not found. Installing Docker..."
                    curl -fsSL https://get.docker.com -o get-docker.sh
                    sudo sh get-docker.sh
                    sudo usermod -aG docker $USER
                    sudo newgrp docker
                else
                    echo "Docker is already installed."
                fi
                '''
            }
        }

        stage('Clone GitHub Repository') {
            steps {
                git branch: 'main', url: "${env.GIT_REPO}"
            }
        }
        
        stage('TF version check') {
            steps {
                    sh 'terraform version'
            }
        }

        stage('TF Initialization') {
            steps {
                script {
                    sh '''
                        terraform init
                    '''
                }
            }
        }
        
        stage('TF Format check') {
            steps {
                script {
                    sh 'terraform fmt -recursive'
                }
            }
        }
        
        stage('TF code validation') {
            steps {
                    sh 'terraform validate'
            }
        }

       stage('TF Plan') {
            steps {
                    sh 'terraform plan'
            }
        }
        
        stage('TF Apply') {
            steps {
                    sh 'terraform apply -auto-approve'
            }
        }
    }

    post {
        success {
            echo "Application deployed successfully."
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}

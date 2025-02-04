pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "bucky0838/finacplus:latest"
        KUBECONFIG_FILE = "kubeconfig.yaml"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Bucky0719/FinacPlus.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker login -u bucky0838 -p dckr_pat_79C2h7PDN21tTnkWp-4-xSNlHIg'
                sh 'docker push $DOCKER_IMAGE'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
               script {
               withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'AWS Cred']]) {
               withKubeConfig(credentialsId: 'kubeconfig', serverUrl: 'https://ADAB8EDA10576EF0A90A4A4118D3B98E.gr7.ap-south-1.eks.amazonaws.com') {
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
                    }  
                  }
                }
            }
        }
    }
}

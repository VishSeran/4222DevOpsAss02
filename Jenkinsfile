pipeline {
    agent any
    
    stages {
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/VishSeran/4222DevOpsAss02.git'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t VishSeran/vads-docker:%BUILD_NUMBER% .'
            }
        }
        
        stage('Login to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'dockerPass')]) {
                    script {
                        bat 'docker login -u seranvishwa -p %dockerPass%'
                    }
                }
            }
        }
        
        stage('Push Image') {
            steps {
                bat 'docker push VishSeran/vads-docker:%BUILD_NUMBER%'
            }
        }
    }
    
    post {
        always {
            bat 'docker logout'
        }
    }
}

pipeline {
    agent any
    environment {
        IMAGE_NAME = "antonios94/web-app-task"
        IMAGE_TAG = "${env.BUILD_ID}"
    }
    
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building..."'
                sh "podman build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }
        stage('Push Docker image to Registry') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub',usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]) {
		  sh "podman login -u ${USERNAME} -p ${PASSWORD} docker.io" 
		  sh "podman push ${IMAGE_NAME}:${IMAGE_TAG}"
		}  
            }
  }
        stage('Test') {
            steps {
                sh 'echo "Testing..."'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }
    }
    

}

pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building..."'
                sh "podman build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
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

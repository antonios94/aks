pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                sh 'echo "Building..."'
                sh 'podman version'
                sh 'kubectl version'
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

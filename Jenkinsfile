pipeline {
    agent any
    environment {
        IMAGE_NAME = "antonios94/web-app-task"
        IMAGE_TAG = "${env.BUILD_ID}"
    }
    
    stages {
        stage('Build web app Docker Image') {
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
        stage('Update Deployment yaml') {
           steps {
                dir('yamls'){
                  sh 'echo "Deploying..."'
                  sh "sed -i 's/nginx:latest/antonios94\\/web-app-task:${IMAGE_TAG}/g' web-app.yaml"
           	}
	   }
  }
        
         stage('Deploy deployment on aks cluster ') {
        steps {
	dir('yamls'){
            withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {                     
        	//sh "kubectl apply -n level5 -f  . --kubeconfig=$KUBECONFIG_FILE"
            }             
        } 
        }
 }
    }
    

}

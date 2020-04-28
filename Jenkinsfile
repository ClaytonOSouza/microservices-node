pipeline {
    environment {
	registry = "claytondevops/nodeservices"
	registryCredential = 'dockerhub'
	dockerImage = ''
    }
    agent any
    stages {
	stage('Cloning our Git') {
	    steps {
	    git 'https://github.com/ClaytonOSouza/microservices-node.git'
            sh 'ls -lrth'
         }
    }
	stage('Building our image') {
	    steps{
	        script {
		    dockerImage = docker.build registry + ":$BUILD_NUMBER"
	            sh 'docker images'
                    sh 'ls -lrth'
              }
	  }	 
    }
	stage('Deploy our image') {
	    steps{
	        script {
	            docker.withRegistry( '', registryCredential ) {
			dockerImage.push()
	              }	
	        }	 	
	  }
    }	
	 stage('Cleaning up') {
	     steps{
	         sh "docker rmi $registry:$BUILD_NUMBER"
		}
	  }
    }
}


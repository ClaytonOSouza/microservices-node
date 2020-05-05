pipeline {
    environment {
        registry = "claytondevops/nodeservices"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
podTemplate(
    label: 'worker', 
    containers: [
        containerTemplate(args: 'cat', name: 'docker', command: '/bin/sh -c', image: 'docker', ttyEnabled: true)
    ],
    volumes: [
      hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
    ]
){
    node('worker') {
    	echo 'Cloning our Git') 
	stage('clone'){
	    git 'https://github.com/ClaytonOSouza/microservices-node.git'
            sh 'ls -lrth'
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
} 

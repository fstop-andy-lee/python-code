node {
    
    agent {
      label "Nodes=JavaSlaveNode1"
    }

    environment {
      DOCKERHUB_TOKEN_ID = 'fstop-andy-lee-dockerhub-token'     
      DOCKER_REGISTRY = 'docker.io'
      DOCKER_NAMESPACE = 'default'
      IMAGE_NAME = 'andylee1973/python'
      CTS = sh(script:'date +%Y-%m-%dT%H:%M:00Z', returnStdout: true).trim()	
      
	  }
  
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {  
       steps {  
        sh 'podman build -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME} .'
        sh 'podman tag ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME} ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:${CTS}'
       }
    }

    //stage('Test image') {        
    //}

    stage('Push image') {
        steps {
               withDockerRegistry([ credentialsId: env.DOCKERHUB_TOKEN_ID, url: "" ]) {
                 sh  'podman push ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:${CTS}'
                 sh  'podman logout'
               }                  
        }
    }
    
    /*
    stage('Trigger ManifestUpdate') {
                echo "triggering update manifest job"
                build job: 'test-python-manifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
    */
}

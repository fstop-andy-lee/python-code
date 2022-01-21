node {

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
       script {
        sh "echo \${DOCKERHUB_TOKEN_ID} "
       }
       
       sh '''
                #!/bin/bash

                # Construct Image Name
                IMAGE=\${env.DOCKERHUB_TOKEN_ID}/\${env.DOCKER_NAMESPACE}/\${env.IMAGE_NAME}
                TAG=\${IMAGE}:\${CTS}

                podman build -t ${IMAGE} .
                podman tag ${IMAGE} ${TAG}                
          '''
    }

    //stage('Test image') {        
    //}

    stage('Push image') {
        steps {
               withDockerRegistry([ credentialsId: env.DOCKERHUB_TOKEN_ID, url: "" ]) {
                 sh  'podman push env.DOCKER_REGISTRY/env.DOCKER_NAMESPACE/env.IMAGE_NAME:env.CTS'
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

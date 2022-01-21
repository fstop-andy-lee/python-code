node {
    
    def  DOCKERHUB_TOKEN_ID = 'fstop-andy-lee-dockerhub-token'     
    def  DOCKER_REGISTRY = 'docker.io'
    def  DOCKER_NAMESPACE = 'default'
    def  IMAGE_NAME = 'andylee1973/python'
  
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {

       sh "echo ${DOCKERHUB_TOKEN_ID} "
       environment {
        CTS = sh(script:'date +%Y-%m-%dT%H:%M:00Z', returnStdout: true).trim()	
        IMAGE = '${DOCKERHUB_TOKEN_ID}/${DOCKER_NAMESPACE}/${IMAGE_NAME}'
        TAG = '${DOCKERHUB_TOKEN_ID}/${DOCKER_NAMESPACE}/${IMAGE_NAME}:${CTS}'
       }
       
       sh """
                #!/bin/bash

                podman build -t \${IMAGE} .
                podman tag \${IMAGE} \${TAG}                
          """
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

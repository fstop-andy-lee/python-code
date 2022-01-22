node {
    
    def  DOCKERHUB_TOKEN_ID = 'fstop-andy-lee-dockerhub-token'     
    def  DOCKER_REGISTRY = 'docker.io'
    def  DOCKER_REGISTRY_URL = 'https://docker.io'
    def  DOCKER_NAMESPACE = 'default'
    def  DOCKER_USER = 'andylee1973'
    def  IMAGE_NAME = 'andylee1973/python'
    def  BUILD_NUMBER = sh(script:'date +%Y-%m-%d', returnStdout: true).trim()  
  
    stage('Clone repository') {
        checkout scm
    }

    stage('Build Container Image') {
       // ${xxx} for jenkins variable, \${xxx} for shell variable
       
       echo "Build Container Image"
       sh "echo ${BUILD_NUMBER} "       
       sh """
                #!/bin/bash
                #IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
                IMAGE=${DOCKER_REGISTRY}/${IMAGE_NAME}
                TAG=\${IMAGE}:${BUILD_NUMBER}
                sudo podman build -t \${IMAGE} .
                sudo podman tag \${IMAGE} \${TAG}                
          """
    }

    //stage('Test image') {        
    //}

    stage('Push Container Image') {
        
        echo "Push Container Image"
        sh "echo ${BUILD_NUMBER} "
        
        /*
        withDockerRegistry( [url: DOCKER_REGISTRY_URL, credentialsId: DOCKERHUB_TOKEN_ID] ) {                
          sh """
              #!/bin/bash
              IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
              IMAGE_WITH_TAG=\${IMAGE}:${BUILD_NUMBER}
              sudo podman push \${IMAGE_WITH_TAG}  
              sudo podman logout ${DOCKER_REGISTRY_URL}
             """
        } 
        */        
        withCredentials([usernamePassword(credentialsId: DOCKERHUB_TOKEN_ID, passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
          sh """
              #!/bin/bash
              #IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
              IMAGE=${DOCKER_REGISTRY}/${IMAGE_NAME}
              IMAGE_WITH_TAG=\${IMAGE}:${BUILD_NUMBER}
              sudo podman login -u \$USERNAME -p \$PASSWORD ${DOCKER_REGISTRY_URL}
              sudo podman push \${IMAGE_WITH_TAG}  
              sudo podman logout ${DOCKER_REGISTRY_URL}
             """
        }  
    }
               
    stage('Trigger Manifest Update') {
        echo "Trigger Manifest Update"
        build job: 'test-python-manifest', parameters: [string(name: 'DOCKERTAG', value: BUILD_NUMBER)]
    }
    
    
}

node {
    
    def  DOCKERHUB_TOKEN_ID = 'fstop-andy-lee-dockerhub-token'     
    def  DOCKER_REGISTRY = 'docker.io'
    def  DOCKER_REGISTRY_URL = 'https://docker.io'
    def  DOCKER_NAMESPACE = 'default'
    def  IMAGE_NAME = 'andylee1973/python'
  
    stage('Clone repository') {
        checkout scm
    }

    stage('Build image') {
       // ${xxx} for jenkins variable, \${xxx} for shell variable
       
       CTS = sh(script:'date +%Y-%m-%d', returnStdout: true).trim()  
       sh "echo ${CTS} "       
       sh """
                #!/bin/bash
                IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
                TAG=\${IMAGE}:${CTS}
                sudo podman build -t \${IMAGE} .
                sudo podman tag \${IMAGE} \${TAG}                
          """
    }

    //stage('Test image') {        
    //}

    stage('Push image') {
        CTS = sh(script:'date +%Y-%m-%d', returnStdout: true).trim()  
        sh "echo ${CTS} " 
        /*
        withDockerRegistry( [url: DOCKER_REGISTRY_URL, credentialsId: DOCKERHUB_TOKEN_ID] ) {                
          sh """
              #!/bin/bash
              IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
              IMAGE_WITH_TAG=\${IMAGE}:${CTS}
              sudo podman push \${IMAGE_WITH_TAG}  
              sudo podman logout
             """
        } 
        */        
        withCredentials([[credentialsId: DOCKERHUB_TOKEN_ID, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh """
              #!/bin/bash
              IMAGE=${DOCKER_REGISTRY}/${DOCKER_NAMESPACE}/${IMAGE_NAME}
              IMAGE_WITH_TAG=\${IMAGE}:${CTS}
              sudo podman login -u $USERNAME -p $PASSWORD ${DOCKER_REGISTRY_URL}
              sudo podman push \${IMAGE_WITH_TAG}  
              sudo podman logout
             """
        }
        
        
    }
    
    /*
    stage('Trigger ManifestUpdate') {
                echo "triggering update manifest job"
                build job: 'test-python-manifest', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
    */
}

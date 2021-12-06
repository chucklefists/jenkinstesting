pipeline {
   agent { 
        docker { 
        image 'mcr.microsoft.com/azure-cli'
        label 'ubuntu-slave'
        args  '-v /tmp:/tmp'
        args  '--user root --privileged'
        } 
   }

   stages {
      stage('Build') {
        steps {
          echo 'Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
          withCredentials([azureServicePrincipal('azuseast')]) {
              sh 'az account list'
              }
        }
   }
   stage('Test') {
     steps {
       sh 'pwd'
       checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'env.WORKSPACE/ansible']], userRemoteConfigs: [[url: 'https://github.com/ansible/ansible-examples.git']]])
       sh 'ls -la env.WORKSPACE/ansible' 
     }
   }
   stage('Deploy') {
     steps {
       echo 'Deploying...'
     }
   }
  }
}

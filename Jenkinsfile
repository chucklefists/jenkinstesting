pipeline {
   agent { 
        docker { 
        image 'mcr.microsoft.com/azure-cli'
        label 'ubuntu-slave'
        args  '-v /tmp:/tmp'
        args  '--user root --privileged'
        } 
   }
   parameters {
        string(name: 'RGNAME', defaultValue: 'Jenkins_Test', description: 'Resource Group Name')
        string(name: 'LOCATION', defaultValue: 'EastUS', description: 'Azure Region')
  }
   stages {
      stage('Build') {
        steps {
          echo 'Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
          withCredentials([azureServicePrincipal('azuseast')]) {
              sh 'az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID'
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

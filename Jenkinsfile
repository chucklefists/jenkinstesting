pipeline {
   agent { 
        docker { 
        image 'maven:3.8.1-adoptopenjdk-11'
        label 'ubuntu-slave'
        args  '-v /tmp:/tmp'
        } 
   }

   stages {
      stage('Build') {
        steps {
          echo 'Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
        }
   }
   stage('Test') {
     steps {
       sh 'pwd'
       checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '/home/jenkins/']], userRemoteConfigs: [[url: 'https://github.com/ansible/ansible-examples.git']]])
       sh 'ls -la /home/jenkins' 
     }
   }
   stage('Deploy') {
     steps {
       echo 'Deploying...'
     }
   }
  }
}

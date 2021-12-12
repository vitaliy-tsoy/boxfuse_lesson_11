pipeline {
  agent {
    docker {
      image 'vittsoy73/agent'
      registryCredentialsId '72245b2a-b07d-41b9-aaa4-676f76897ddf'
      args '-v /var/run/docker.sock:/var/run/docker.sock -u root'
    }
  }
  stages {
    stage('Checkout code') {
      steps {
        git 'https://github.com/vitaliy-tsoy/boxfuse_lesson_11.git'
      }
    }
    stage('Build') {
      steps {
         sh "mvn package"
      }
    }
    stage('Build And Push Docker Image') {
      steps {
        sh '/usr/bin/docker build -t app_image .'
        sh '/usr/bin/docker tag app_image vittsoy73/boxfuse_app:2.0.0'
        sh '/usr/bin/docker push vittsoy73/boxfuse_app:2.0.0'
      }
    }
    stage('Run docker on prod-server') {
      steps {
             sh 'ssh-keyscan -H prod-server >> ~/.ssh/known_hosts'
             sh '''ssh root@prod-server << EOF 
             docker run -d -p 8081:8080 vitaliy-tsoy/boxfuse_app:2.0.0
             exit
             EOF'''
      }
    }
  }
}

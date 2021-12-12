pipeline {
  agent {
    docker {
      image 'vitaliy-tsoy/agent'
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
        sh 'docker build -t app_image .'
        sh 'docker tag app_image vitaliy-tsoy/boxfuse_app:2.0.0'
        sh  'docker push vitaliy-tsoy/boxfuse_app:2.0.0'
      }
    }
    stage('Run docker on instance-3') {
      steps {
             sh 'ssh-keyscan -H instance-3 >> ~/.ssh/known_hosts'
             sh """ssh root@instance-3<< EOF 
             docker run -d -p 8081:8080 vitaliy-tsoy/boxfuse_app:2.0.0
             exit
             EOF"""
      }
    }
  }
}

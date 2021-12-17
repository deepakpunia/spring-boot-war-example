pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Test') {
      steps {
        sh 'mvn test'
        slackSend(channel: 'youtubejenkins', message: 'Job Started')
      }
    }

    stage('Build') {
      steps {
        sh 'mvn package'
      }
    }

    stage('Deploy on Test') {
      steps {
        deploy(adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://192.168.0.118:8080')], contextPath: '/app', war: '**/*.war')
      }
    }

    stage('Deploy on Prod') {
      input {
        message 'Should we continue?'
        id 'Yes we Should'
      }
      steps {
        deploy(adapters: [tomcat9(credentialsId: 'tomcatserverdetails1', path: '', url: 'http://192.168.0.119:8080')], contextPath: '/app', war: '**/*.war')
      }
    }

  }
  tools {
    maven 'Maven'
  }
  post {
    always {
      echo '========always========'
    }

    success {
      echo '========pipeline executed successfully ========'
      slackSend(channel: 'youtubejenkins', message: 'Success')
    }

    failure {
      echo '========pipeline execution failed========'
      slackSend(channel: 'youtubejenkins', message: 'Job Failed')
    }

  }
}
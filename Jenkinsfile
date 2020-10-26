#!groovy​
// Pipeline Defination.
pipeline {
  agent any
  triggers {
    pollSCM('H/2 * * * *')
  }
  stages {   
    stage('Build to Production') {
      when { branch "master" }
      post {
          always {
              slackSend(color: 'good', message: "Production Build Started on ${env.JOB_NAME} (<${env.BUILD_URL}|See details here>)")
          }
      }
      steps {
        sh 'mvn package'
      }
    }

    stage('Delivery to Production') {
      when { branch "master" }
      steps {
        echo '====++++executing Deliver for development++++===='
        winRMClient credentialsId: 'c1c51fc9-786f-404c-b629-1a01e1203758', hostName: 'w-wagendamentos.clinicasim.local', winRMOperations: [sendFile(configurationName: 'DataNoLimits', destination: 'C:\\Webs\\Prod\\chatbot.clinicasim.com\\new_version', source: "${env.WORKSPACE}\\target\\demo-0.0.1.war")]    
      }
    }

  }  

  post {
    success {
      slackSend(color: 'good', message: "Build success on ${env.JOB_NAME} (<${env.BUILD_URL}|See details here>)")
      cleanWs()
    }

    failure {
      slackSend(color: 'ff0000', message: "Build fail on ${env.JOB_NAME} (<${env.BUILD_URL}|See details here>)")
      cleanWs()
    }

  }  
}
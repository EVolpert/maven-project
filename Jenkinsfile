pipeline {
  agent any

  tools {
          maven 'localMaven'
      }

  stages {
    stage('Build'){
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
          echo 'Now Archiving...'
          archiveArtifacts artifacts: '**/target/*.war'
        }
      }
    }
    stage('Deploy to Staging') {
      steps {
        build job: 'maven-project-deploy-to-staging'
      }
    }
    stage('Deploy to Prod') {
      steps {
        timeout(time:5, unit:'DAYS'){
          input message: 'Approve PROD Deploy?'
        }

        build job: 'maven-project-deploy-to-prod'
      }
      post {
        success {
          echo 'Code deployed to Prod'
        }
        failure {
          echo 'Deploy failed'
        }
      }
    }
  }
}

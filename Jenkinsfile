pipeline {
  agent any 

  environment{
    DOKCER_HUB_CREDENTIALS = credentials('mentoring-dockerhub')
  }

  stages {
    stage("verify tooling") {
      steps {
        sh '''
          docker -v
          docker compose -v
           '''
      }
    }
    
    stage("build") {  
      steps {
        // sh 'docker compose docker-compose.yml build'
        sh "docker build -t api-nest:latest apps/api/ "
        sh "docker build -t api-next:latest apps/web/"
      }
    }

    stage('login Docker hub') {
      steps {
        withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
            sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
        }
      }
    }

    stage('tag docker image '){
      steps{
        sh 'docker tag api-next:latest ikhela/mentoring:api-next'
        sh 'docker tag api-nest:latest ikhela/mentoring:api-nest'
      }
    }

    stage('push docker image '){
      steps{
        sh 'docker push ikhela/mentoring:api-next'
        sh 'docker push ikhela/mentoring:api-nest'
      }
    }

    stage('Clean') {
      steps {
        // Supprimer les images locales non utilisées
        sh 'docker image rm api-nest:latest'
        sh 'docker image rm app-next:latest'
        sh "docker logout"
      }
    }
  
    stage('Email Notification'){
      steps {
        script{
          // Send email notification 
          mail to: 'kankoffi36@gmail.com', 
                subject: "Jenkins Build Notification",
                body: "Pipeline: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n\n${BUILD_URL}\n\n Succes build and push image"
        }
      }
    }
  }
}

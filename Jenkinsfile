pipeline {
  agent any 
  tools{
        jdk 'jdk17'
        nodejs 'node20'
  }
  environment{
    DOCKER_HUB_CREDENTIALS = credentials('docker-crd')
    EMAIL_ADDRESS = "kankoffi36@gmail.com"
    REPOSITORY_DOCKER_HUB =" ikhela/mentoring"
    SCANNER_HOME=tool 'sonar-scanner'
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
    stage("Sonarqube Analysis "){
        steps{
            withSonarQubeEnv('sonar-server') {
                  sh '''${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=Mentoring \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://172.25.66.200:9000 \
                        -Dsonar.token=sqp_8104dfca8e8b1cddde4707399222741022708ce8
                    '''
            }
        }
    }
    stage("quality gate"){
      steps {
            script {
                waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
            }
        } 
    }
    // install dependencies 

    // for owasp scan 
    stage("OWASP FS SCAN"){
      steps{
        dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
        dependencyCheckPublish pattern: '**/dependency-check-report.xml'
      }
    }

    // add trivy Scan for image 
    stage('TRIVY FS SCAN'){
      steps{
        sh 'trivy fs . > trivyfs.txt'
      }
    }
    
    stage("build") {  
      steps {
        sh 'docker compose -f docker-compose.yml build'
      }
    }

    stage('login Docker hub') {
      steps {
        sh 'docker login -u $DOCKER_HUB_CREDENTIALS_USR -p $DOCKER_HUB_CREDENTIALS_PSW'
      }
    }

    stage('tag docker image '){
      steps{
        sh 'docker tag web-next:latest ikhela/mentoring:web-next'
        sh 'docker tag api-nest:latest ikhela/mentoring:api-nest'
      }
    }

    // scan trivy image 
    stage('Trivy scan image'){
      steps{
        sh 'trivy --no-progress --exit-code 1 severity HIGH,CRITICAL ikhela/mentoring:web-next'
        sh 'trivy --no-progress --exit-code 1 severity HIGH,CRITICAL ikhela/mentoring:api-nest'
      }
    }
    
    stage('push docker image '){
      steps{
        sh 'docker push ikhela/mentoring:web-next'
        sh 'docker push ikhela/mentoring:api-nest'
      }
    }

    // stage('Clean') {
    //   steps {
    //     // Supprimer les images locales non utilisées
    //     sh 'docker image rm api-nest:latest'
    //     sh 'docker image rm web-next:latest'
    //     sh "docker logout"
    //   }
    // }
    stage("Deploy to container") {  
      steps {
        sh 'docker compose -f docker-compose.yml up -d'
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

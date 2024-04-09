pipeline {
  agent any
  
  stages {
    stage('Checkout') {
      steps {
        sh 'echo passed'
        git branch: 'main', url: 'https://github.com/amanyashu/Project7---Deploying-a-Java-Web-Application-into-Kubernetes-Cluster-using-Ansible.git'
      }
    }
    stage('Build and Test') {
      steps {
        sh 'ls -ltr'
        // build the project and create a JAR file
        sh 'mvn clean package'
        //sh 'cd spring-boot-app && java -jar target/spring-boot-web.jar'
       // sh 'echo $! > pid.txt'
      }
    }
    stage('Static Code Analysis') {
      environment {
        SONAR_URL = "http://40.112.138.150:9000/"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
          sh 'mvn sonar:sonar -Dsonar.login=$SONAR_AUTH_TOKEN -Dsonar.host.url=${SONAR_URL}'
        }
      }
    }
   
   }
    stage('Build and Push Docker Image') {
      environment {
       DOCKER_IMAGE = "sahanasonu272/amanimage:${BUILD_NUMBER}"
        REGISTRY_CREDENTIALS = credentials('docker-cred')
      }
      steps {
        script {
            sh 'docker build -t ${DOCKER_IMAGE} .'
            def dockerImage = docker.image("${DOCKER_IMAGE}")
            docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
            dockerImage.push()
            }
        }
      }
    }
   
}

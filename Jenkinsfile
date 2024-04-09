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
}

    
    stage('Static Code Analysis') {
      environment {
        SONAR_URL = "http://40.118.241.12:9000/"
      }
      steps {
        withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
          sh 'mvn clean verify sonar:sonar \
  -Dsonar.projectKey=MavenProject \
  -Dsonar.host.url=http://40.118.241.12:9000 \
  -Dsonar.login=sqp_8fd644a1f2be73344320e296e1d3884c05c5b3e4'
        }
      }
    }
   
   

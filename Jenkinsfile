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
   
    //stage('Stop') {
     // steps {
       // sh 'cd spring-boot-app && java -jar target/spring-boot-web.jar'
        //sh 'echo $! > pid.txt'
        //sh 'curl -X POST http://52.167.200.192:9999/actuator/shutdown'
        //sh 'rm pid.txt'
      //}
  //}
//stage ('Deploy') {
    
   // steps {
      //  echo "deploy stage"
        //sh "curl -X POST -u \${manager}:\${manager_password} http://52.167.200.192:8088/manager/text/deploy?path=/test&war=file:./target/*.jar"
    //    sh "curl -X POST -u manager:manager_password http://52.167.200.192:8088/manager/text/deploy?path=/test&war=file:./target/*.jar"
  //  }
//}
//stage ('Deploy') {

        //steps {
      //      echo "deploy stage"
            //sh "curl -X POST -u manager:manager_password http://52.167.200.192:8088/manager/text/deploy?path=/test&war=:**/*.war"
            //deploy adapters: [tomcat9 (
              //      credentialsId: 'tomcat_deploy_credentials',
                //    path: '',
                  //  url: 'http://http://40.112.138.47:8088/'
                //)],
                //contextPath: 'test',
                //onFailure: 'false',
                //war: '**/*.war'
        //}
    //}

//stage('Deploy to Tomcat') {
  //          steps {
    //            script {
      //              def tomcatUrl = 'http://52.167.200.192:8088'
        //            def credentialsId = 'tomcat_deploy_credentials'
          //          def contextPath = '/myapp' // Adjust as needed
            //        sh 'cd spring-boot-app && sudo cp target/spring-boot-app.war /opt/tomcat/latest/webapps/'
                    //def warFile = sh(script: 'ls spring-boot-app/target/*.jar', returnStdout: true).trim()
                    //def deployCmd = "curl -u war-deployer:jenkins-tomcat-deploy -T ${warFile} ${tomcatUrl}/manager/text/deploy?path=${contextPath}"

                    //sh deployCmd
              //  }
            //}
//}
    stage('Build and Push Docker Image') {
      environment {
       DOCKER_IMAGE = "sahanasonu272/spring-boot-app:${BUILD_NUMBER}"
         DOCKERFILE_LOCATION = "/Dockerfile"
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

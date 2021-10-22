pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image 'maven:3.8.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image 'maven:3.8.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image 'maven:3.8.3-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Docker BnP') {
      agent any
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("roronoasrinu/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

      }
    }
    stage('Deploy to Dev'){
         when{
           beforeAgent true
          branch 'master'
         }
         agent any
         steps{
           echo 'Deploting to dev environment with Docker compose.'
           sh 'docker-compose up -d'
         }
    }
  }
  tools {
    maven 'Maven'
  }
  post {
    always {
      echo 'This pipeline is completed..'
    }

  }
}

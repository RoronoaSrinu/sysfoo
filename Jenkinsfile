pipeline {
  agent none
  stages {
    stage('build') {
      agent {
        docker {
          image '3.8.3:-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn compile'
      }
    }

    stage('test') {
      agent {
        docker {
          image '3.8.3:-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn clean test'
      }
    }

    stage('package') {
      agent {
        docker {
          image '3.8.3:-jdk-11-slim'
        }

      }
      steps {
        sh 'mvn package -DskipTests'
        archiveArtifacts 'target/*.war'
      }
    }

    stage('Docker BnP') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', 'dockerlogin') {
            def dockerImage = docker.build("RoronoaSrinu/sysfoo:v${env.BUILD_ID}", "./")
            dockerImage.push()
            dockerImage.push("latest")
            dockerImage.push("dev")
          }
        }

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
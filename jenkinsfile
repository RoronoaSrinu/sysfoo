pipeline {
  agent any
  tools{
      maven 'Maven'
  }
  stages{
      stage("build"){
          steps{
              sh 'mvn compile'
          }
      }
      stage("test"){
          steps{
              sh 'mvn clean test'
          }
      }
      stage("package"){
          steps{
             sh 'mvn package-dskipTests'
          }
      }
  }

  post{
    always{
        echo 'This pipeline is completed..'
    }
  }
}
pipeline {
    agent any

    tools {
        // auto installed maven
        maven "MAVEN_HOME"
    }

    stages {
        stage('cleanup')
        {
            steps{
                deleteDir()
            }
        }
        stage('Git Checkout') {
            steps {
                
                git credentialsId: 'dharmareddy', url: 'https://github.com/dharmawip/devops-app'
            }

        }  
        stage('maven and jacoco') {
            steps {
                sh 'mvn clean install'
                //sh 'mvn sonar:sonar'
            }
         }
       stage('Sonarqube') {
    environment {
        scannerHome = tool 'Sonar'
    }
        steps {
        withSonarQubeEnv('SonarServer') {
            sh "sonar.projectVersion=1.0
sonar.sources=src/main/java

sonar.binaries=target/classes "
            //sh "${scannerHome}/bin/sonar-scanner"
          echo 'some'
        }
        }
    }
       
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubetoken'
               echo 'some'
            }
        }
    
        stage('Deploy') {
            steps {
                //sh 'mvn test'
                echo 'deploying to nexus of target server'
            }

        }
    }
}

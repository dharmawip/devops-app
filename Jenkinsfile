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
                
                git credentialsId: 'dharmareddy', url: ''
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
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=maven-git -Dsonar.sources=src/myappmain -Dsonar.java.binaries=target/classes/src/myappmain/ "
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

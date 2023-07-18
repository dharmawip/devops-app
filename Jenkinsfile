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
            environment {
            PATH= "c:\Users\user\maven\apache-maven-3.8.8\bin:$PATH"
            steps {
                sh "mvn clean install"
                //sh 'mvn sonar:sonar'
            }
         }
       stage('Sonarqube') {
   
        scannerHome = tool 'Sonar'
    }
        steps {
        withSonarQubeEnv('SonarServer') {
            sh "${scannerHome}/bin/sonar-scanner D_sonar.projectVersion=1.0 D_sonar.sources=src/main/java D_sonar.binaries=target/classes "
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

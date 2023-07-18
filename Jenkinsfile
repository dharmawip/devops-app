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
       
       stage('Sonarqube') {
            environment {
            PATH= "M2_HOME:$PATH"
                scannerHome = tool 'Sonar'
            }
           steps {
               withSonarQubeEnv('SonarServer') {
                   sh "$C:/Users/user/maven/apache-maven-3.8.8/bin/mvn sonar.projectVersion=1.0 sonar.sources=src/main/java"
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

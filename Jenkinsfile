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
                   mvn clean verify sonar:sonar \
                   -Dsonar.projectKey=maven-git \
                   -Dsonar.host.url=http://192.168.43.157:9000 \
                   -Dsonar.login=sqp_367cb43584649a44cbc618e61af59f2564d22dc4
                   echo 'some'
               }
           }
        stage("Quality gate") {
            steps {
                waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubejenkin'
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

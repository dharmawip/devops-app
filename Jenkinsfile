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
        stage('SonarQube Analysis') {
               withSonarQubeEnv {
                   sh """
                   -D sonar.login=admin \
                   -D sonar.password=DHA@rma456 \
                   -D sonar.language=java \
                   -D sonar.sources=devops-app/Demo1/src/main/java/com/devops/App.java\
                   -D sonar.tests=devops-app/Demo1/src/test/java/com/devopstest/AppTest.java\
                   -Dsonar.projectKey=maven-git \
                   -Dsonar.host.url=http://192.168.43.157:9000 \
                   -Dsonar.login=sqp_367cb43584649a44cbc618e61af59f2564d22dc4"""
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

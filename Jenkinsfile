


pipeline {
    agent any
    

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('sonar-token')
        SONAR_ORGANIZATION = 'customer-service-ai-chatbot'
        SONAR_PROJECT_KEY = 'customer-service-ai-chatbot_ci-jenkins'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
  -Dsonar.organization=customer-service-ai-chatbot \
  -Dsonar.projectKey=customer-service-ai-chatbot_ci-jenkins \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io'''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'docker-cred') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("joseph151/crud-application:latest")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                        sh 'docker rm -f $(docker ps -q) || true'
                        sh 'docker run -d -p 3000:3000 joseph151/crud-application:latest'
                        
                    
                }
            }
        }
        
}
}

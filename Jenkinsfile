pipeline {
    agent any 
     environment {
                   key = credentials('openAI')
                }
    stages {
        stage('Code fetch') {
            steps {
                git url:'https://github.com/Bandank/chatgpt-web-application.git',branch:'master'
            }
        }
        stage('build and test') {
            steps {
                sh 'docker build . -t iamadminallowme/chatgpt-app:latest'
            }
        }
        stage('push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push iamadminallowme/chatgpt-app:latest'
            }
          }
        }
        stage('Deploy'){
            steps {
                withCredentials([string(credentialsId: 'openAI', variable: 'OPENAI_API_KEY')]) {
                                sh 'docker-compose down'
                                sh "docker-compose up -d"
                        }
                    
            }
        }
    }
}

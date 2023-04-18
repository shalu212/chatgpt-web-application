pipeline {
    agent {label 'Devops_Agent'}
    stages {
        stage('Code fetch') {
            steps {
                git url:'https://github.com/Bandank/chatgpt-web-application.git',branch:'master'
            }
        }
        stage('build and test') {
            steps {
                sh 'sudo docker build . -t bandank/chatgpt-app:latest'
            }
        }
        stage('push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                sh 'docker push bandank/chatgpt-app:latest'
            }
          }
        }
        stage('Deploy'){
            environment {
                   key = credentials('openAI')         
                }
            steps {
                    sh 'docker-compose down'
                    sh "docker-compose up -d -e $key_secret"
            }
        }
    }
}

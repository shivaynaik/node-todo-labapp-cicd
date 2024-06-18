pipeline {
    agent { label 'docker-agent' }
    
    stages{
        stage('code'){
            steps {
                git url: 'https://github.com/shivaynaik/node-todo-labapp-cicd.git', branch: 'main'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'docker build . -t shivaynaik/node-todo-labapp-cicd:latest'
            }
        }
        stage('Login and Push Image'){
            steps {
                echo 'logging in to docker hub and pushing image..'
                withCredentials([usernamePassword(credentialsId:'dockerhub',passwordVariable:'DockerHubPassword', usernameVariable:'DockerHubUsername')]) {
                    sh 'docker login -u ${env.DockerHubUsername} -p ${env.DockerHubPassword}'
                    sh "docker push shivaynaik/node-todo-labapp-cicd:latest"
                }    
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}

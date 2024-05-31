pipeline {
    agent any
    stages{
        stage("code clone"){
            steps{
                git url: "https://github.com/VahiniPonnoju/django-to-app.git", branch: "main"
                echo"Code Cloned"
            }
        }
        stage("Code Build"){
            steps{
                sh "docker build . -t django-app "
                echo"Code Build successfully"
            }
        }
        stage("Pushing the Imaage"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker tag django-app:latest ${env.dockerhubuser}/django-app:latest"
                sh "docker push ${env.dockerhubuser}/django-app:latest"
                }
                echo"Pushing image to the docker hub"
            }
        }
        stage("Deploying the Application"){
            steps{
                sh "docker compose down && docker compose up -d"
                echo"Application got deployed"
            }
        }
    }
}

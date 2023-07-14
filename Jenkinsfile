pipeline {
    agent any
    stages{
        stage("Clone Code"){
          steps{
             echo "Cloning the code"
             git url: "https://github.com/Manish-Syal123/node-todo-cicd.git", branch: "master"
          }
        }
        stage("Build and Test"){
            steps{
                echo "Building the image"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                  sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
                  sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                  sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
                
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

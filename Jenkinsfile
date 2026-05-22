pipeline{
    agent any
    stages{
        stage("code"){
            steps {
                echo "clonig the code"
                git url: "https://github.com/LondheShubham153/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps {
                echo "building the code"
                sh "docker build -t my-note-app ."
            }
        }
        stage("push to the docker hub"){
            steps{
                echo "pushing the image to the Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("deploy"){
            steps {
                echo "deploying the container"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}

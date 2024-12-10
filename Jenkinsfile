pipeline {
    agent any
    
    stages{
        stage("code"){
            steps {
                echo "Cloning the code"
                git url :"https://github.com/LondheShubham153/django-notes-app.git", branch:"main"
            }
            
        }
        stage("build"){
            steps {
                echo "Building the code"
                sh "docker build -t node-app ."
            }
            
        }
        stage("Push to Docker Hub"){
            steps {
                echo "pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId:"dockerhub", passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")])
               {
                   sh "docker tag note-app ${env.dockerhubUser}/node-app:latest"
                  sh '''
                echo "${dockerhubPass}" | docker login -u "${dockerhubUser}" --password-stdin
            '''
                   sh "docker push ${env.dockerhubUser}/node-app:latest"
               }
            }
            
        }
        stage ("Deploy"){
            steps {
                echo "deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}

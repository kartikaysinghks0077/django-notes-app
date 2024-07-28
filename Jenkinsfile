pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/kartikaysinghks0077/django-notes-app.git", branch: "main"
            }
        }
        
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-app ."
            }
        }
        
        stage("Push to docker hub") {
            steps {
                echo "Pushing the image to docker hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubPass", usernameVariable: "dockerhubUser")]) {
                    sh "docker tag my-app ${env.dockerhubUser}/my-app:latest"
                    
                    sh 'echo $dockerhubPass | docker login -u $dockerhubUser --password-stdin'
                    
                    sh "docker push ${env.dockerhubUser}/my-app:latest"
                    
                }
            }
        }
        
        stage("Deploy") {
            steps {
                echo "Deploying the code"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

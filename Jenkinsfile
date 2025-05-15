pipeline {
    agent any

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/idreesahmed396/two-tier-flask-app.git", branch: "master"
                echo "Code clone ho gaya...."
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
                echo "Docker Build bhi ho gaya"
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh ke dega..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {
                    sh 'docker login -u $dockerHubUser -p $dockerHubPass'
                    sh 'docker tag two-tier-flask-app $dockerHubUser/two-tier-flask-app:latest'
                    sh 'docker push $dockerHubUser/two-tier-flask-app:latest'
                    echo "Docker image push bhi ho gaya..."
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker-compose up -d --build flask-app"
                
                echo "Docker Compose se deploy bhi ho gaya..."
            }
        }
    }
}

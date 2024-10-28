pipeline {
        agent any

        stages {
            stage('Clone Code') {
                steps {
                    echo 'Cloning the code'

                    git url: "https://github.com/MourtGit/notes-app.git", branch: "main"
                }
            }
            stage('Build') {
                steps {
                    echo 'This is Build Stage'
                    sh "docker build -t notes-app ."
                }
            }
            stage('Push to Docker hub') {
                steps {
                    echo 'Pushing image to dockerhub'
                    withCredentials([usernamePassword(credentialsId:"dockerhub-login",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser" )]){
                    sh "docker tag notes-app ${env.dockerhubuser}/notes-app:latest"
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}" 
                    sh "docker push ${env.dockerhubuser}/notes-app:latest"
                    }

                }
            }
            stage('Deployement') {
                steps {
                    echo 'Deploying container'
                    sh "docker-compose down && docker-compose up -d"
                }
            }
        }
    }

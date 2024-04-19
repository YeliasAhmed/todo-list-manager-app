pipeline {
    agent any
    
    stages {
        
        stage("Code Checkout"){
            steps {
                git branch: 'main', url: 'https://github.com/YeliasAhmed/todo-list-manager-app.git'
            }
        }
        
        stage('Docker Image Build'){
            steps {
                sh 'docker image build -t yeliasahmed/todolist:v$BUILD_ID .'
                sh 'docker image tag yeliasahmed/todolist:v$BUILD_ID yeliasahmed/todolist:latest'
            }
        }
        
        stage('Push Dokcer Image'){
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'pass', usernameVariable: 'user')]) {
                    sh 'docker login -u ${user} -p ${pass}'
                    sh 'docker push yeliasahmed/todolist:v$BUILD_ID'
                    sh 'docker push yeliasahmed/todolist:latest'
                    sh 'docker image rmi yeliasahmed/todolist:v$BUILD_ID yeliasahmed/todolist:latest'
                    
               }
            }
        }
        
        stage('Creating Docker Container'){
            steps {
                sh 'docker run -itd --name todolist -p 3000:3000 yeliasahmed/todolist:latest'
            }
        }
    }
}
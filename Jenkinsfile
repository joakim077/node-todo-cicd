pipeline {
    agent {label 'runner_01'}

    stages {
        stage('Checkout') {
            steps {
               git branch: 'master', url: 'https://github.com/joakim077/node-todo-cicd.git'
            }
        }
        stage('build') {
            steps {
               sh 'docker build -t node-todo-app .'
            }
        }
        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-cred', passwordVariable: 'PASSWD', usernameVariable: 'USERNAME')]) {
                    sh "docker tag node-todo-app ${USERNAME}/node-todo-app"
                    sh "docker login -u ${USERNAME} -p${env.PASSWD} "
                    sh "docker push ${USERNAME}/node-todo-app "
                }
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker compose up -d '
            }
        }
    }
}

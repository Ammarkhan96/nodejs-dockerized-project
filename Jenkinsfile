pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }

        stage("Test") {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }

        stage("Build") {
            steps {
                sh 'npm run build'
            }
        }

        stage("Build Image") {
            steps {
                sh 'docker build -t my-node-app:1.0 .'
            }
        }

        stage("Docker Push") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker_cred',
                    usernameVariable: 'DOCKERHUB_USERNAME',
                    passwordVariable: 'DOCKERHUB_PASSWORD'
                )]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker tag my-node-app:1.0 ammarkhan96/my-node-app:1.0'
                    sh 'docker push ammarkhan96/my-node-app:1.0'
                    sh 'docker logout'
                }
            }
        }
    }
}

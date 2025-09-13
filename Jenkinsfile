pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        REPOSITORY_URI = 'Sareenh1/devops-task-app'
        ECS_CLUSTER = 'devops-task-cluster'
        ECS_SERVICE = 'devops-task-service'
    }
    stages {
        stage('Checkout') {
            steps {
                git(
                    branch: 'main', 
                    url: 'https://github.com/Sareenh1/devops-task.git',
                    credentialsId: 'github-credentials'
                )
            }
        }
        // Rest of your stages...
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${REPOSITORY_URI}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }
}

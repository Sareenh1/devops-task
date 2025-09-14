pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        REPOSITORY_URI = 'your-dockerhub-username/devops-task-app'
        ECS_CLUSTER = 'devops-task-cluster'
        ECS_SERVICE = 'devops-task-service'
        ECS_TASK_DEFINITION = 'devops-task-task-def'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Try to checkout main branch first, fallback to master if needed
                    try {
                        git branch: 'main', 
                             url: 'https://github.com/Sareenh1/devops-task.git',
                             credentialsId: 'github-credentials'
                    } catch (Exception e) {
                        echo "Main branch not found, trying master branch"
                        git branch: 'master', 
                             url: 'https://github.com/Sareenh1/devops-task.git',
                             credentialsId: 'github-credentials'
                    }
                }
            }
        }
        stage('Build and Test') {
            steps {
                sh 'npm install'
                // If you don't have tests yet, you can skip this or add a simple test
                sh 'echo "Tests would run here"'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${REPOSITORY_URI}:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${REPOSITORY_URI}:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy to ECS') {
            steps {
                script {
                    // For now, just echo the deployment command until we fix the checkout issue
                    sh """
                    echo "Would deploy to ECS here"
                    echo "aws ecs update-service --cluster ${ECS_CLUSTER} --service ${ECS_SERVICE} --force-new-deployment"
                    """
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}

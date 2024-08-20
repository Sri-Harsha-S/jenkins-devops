#https://github.com/Sri-Harsha-S/Jenkins_devops_exams.git

pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_credentials')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.build("your_dockerhub_username/your_image_name:${env.BUILD_NUMBER}")
                }
            }
        }
        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'DOCKERHUB_CREDENTIALS') {
                        docker.image("your_dockerhub_username/your_image_name:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def environment = params.Environment
                    if (environment != 'prod' || BRANCH_NAME == 'master') {
                        sh "kubectl apply -f k8s/${environment}/deployment.yaml"
                    }
                }
            }
        }
    }
}


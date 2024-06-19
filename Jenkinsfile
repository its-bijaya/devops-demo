pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'dockerhub-credentials-id' // Replace with your DockerHub credentials ID
        AWS_CREDENTIALS_ID = 'aws-credentials-id' // Replace with your AWS credentials ID
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    def customImage = docker.build("devops-demo", "./app")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image("devops-demo").inside {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER'), 
                                     string(credentialsId: AWS_CREDENTIALS_ID, variable: 'AWS_ACCESS_KEY_ID'), 
                                     string(credentialsId: AWS_CREDENTIALS_ID, variable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh 'docker login -u $DOCKER_USER -p $DOCKER_PASS'
                        sh 'docker tag devops-demo your-dockerhub-username/devops-demo:latest'
                        sh 'docker push your-dockerhub-username/devops-demo:latest'
                        sh 'aws ecs update-service --cluster your-cluster-name --service your-service-name --force-new-deployment'
                    }
                }
            }
        }
    }
}

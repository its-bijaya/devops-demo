pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    def customImage = docker.build("devops-demo")
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
                    withCredentials([string(credentialsId: 'aws_access_key_id', variable: 'AWS_ACCESS_KEY_ID'), string(credentialsId: 'aws_secret_access_key', variable: 'AWS_SECRET_ACCESS_KEY')]) {
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


pipeline {
    agent any

    stages {
        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], // Ensure this matches your branch name
                    userRemoteConfigs: [[
                        url: 'https://github.com/itstopsycret/devops-demo.git',
                        credentialsId: 'github-credentials' // Replace with actual credentials ID
                    ]]
                ])
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t devops-demo .'
            }
        }

        // Add more stages as needed (test, deploy, etc.)
    }
}

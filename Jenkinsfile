
pipeline {
    agent any

    stages {

        stage('GitHub Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-git-credentials',
                    url: 'https://github.com/Mukesh1995singh/jenkins-learning.git'
            }
        }

        stage('Verify Source Code') {
            steps {
                bat 'echo Checking repository files'
                bat 'dir'
                bat 'type app.txt'
            }
        }
    }

    post {
        success {
            echo 'GitHub checkout completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}

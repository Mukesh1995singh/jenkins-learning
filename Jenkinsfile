
pipeline {
    agent any

    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['Dev', 'Test', 'Prod'],
            description: 'Select deployment environment'
        )
    }

    stages {

        stage('GitHub Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-git-credentials',
                    url: 'https://github.com/Mukesh1995singh/jenkins-learning.git'
            }
        }

        stage('Show Environment') {
            steps {
                echo "Selected Environment: ${params.ENVIRONMENT}"
            }
        }

        stage('Show Jenkins Variables') {
            steps {
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Build ID: ${env.BUILD_ID}"
                echo "Workspace: ${env.WORKSPACE}"
                echo "Build URL: ${env.BUILD_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Building application'
                bat 'echo Build completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application'
                bat 'echo Test completed'
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
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}

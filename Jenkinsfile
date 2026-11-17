
@Library('my-shared-library') _

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
                git(
                    url: 'https://github.com/Mukesh1995singh/jenkins-learning.git',
                    branch: 'main',
                    credentialsId: 'github-git-credentials'
                )
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

                catchError(
                    buildResult: 'SUCCESS',
                    stageResult: 'FAILURE'
                ) {
                    bat 'exit /b 1'
                }
            }
        }

        stage('Create Artifact') {
            steps {
                echo 'Creating application artifact'

                bat 'echo This is my application artifact > app-build.txt'

                archiveArtifacts(
                    artifacts: 'app-build.txt',
                    fingerprint: true
                )
            }
        }

        stage('Test') {
            steps {
                echo 'Testing application'
                bat 'echo Test completed'
            }
        }

        stage('Parallel Checks') {
            parallel {

                stage('Code Quality Check') {
                    steps {
                        echo 'Running Code Quality Check'
                        bat 'echo Code Quality Check completed'
                    }
                }

                stage('Security Scan') {
                    steps {
                        echo 'Running Security Scan'
                        bat 'echo Security Scan completed'
                    }
                }
            }
        }

        stage('Verify Source Code') {
            steps {
                bat 'echo Checking repository files'
                bat 'dir'
                bat 'type app.txt'
            }
        }

        stage('Shared Library Demo') {
            steps {
                hello()
            }
        }

        stage('Production Approval') {
            when {
                expression {
                    params.ENVIRONMENT == 'Prod'
                }
            }

            steps {
                input(
                    message: 'Approve Production deployment?',
                    ok: 'Approve'
                )
            }
        }

        stage('Deploy') {
            when {
                expression {
                    params.ENVIRONMENT == 'Prod'
                }
            }

            steps {
                echo "Deploying application to ${params.ENVIRONMENT}"
                bat 'echo Production deployment completed'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed'
        }

        success {
            echo 'Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed! Check console output.'
        }

        unstable {
            echo 'Pipeline is unstable!'
        }
    }
}

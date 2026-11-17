
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

    environment {
        APP_NAME = 'Jenkins Learning'
        BUILD_TYPE = 'Demo'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub'

                git(
                    url: 'https://github.com/Mukesh1995singh/jenkins-learning.git',
                    branch: 'main',
                    credentialsId: 'github-git-credentials'
                )

                bat 'dir'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${APP_NAME}"
                echo "Environment: ${params.ENVIRONMENT}"
                bat 'echo Application build completed'
            }
        }

        stage('Test') {
            steps {
                echo 'Running application tests'
                bat 'echo Tests completed successfully'
            }
        }

        stage('Environment Variables') {
            steps {
                echo "Job Name: ${env.JOB_NAME}"
                echo "Build Number: ${env.BUILD_NUMBER}"
                echo "Workspace: ${env.WORKSPACE}"
                echo "Build Type: ${env.BUILD_TYPE}"
            }
        }

        stage('Failure Handling Demo') {
            steps {
                echo 'Failure handling lesson completed'
            }
        }

        stage('Conditional Deployment') {
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

                echo 'Production deployment approved'
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

        stage('Shared Library Demo') {
            steps {
                hello()
            }
        }

        // Lesson 20: Credentials Binding
        stage('Credentials Binding Demo') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'github-git-credentials',
                        usernameVariable: 'GITHUB_USERNAME',
                        passwordVariable: 'GITHUB_TOKEN'
                    )
                ]) {
                    bat '''
                        if defined GITHUB_USERNAME (
                            echo GitHub username loaded successfully
                        ) else (
                            echo GitHub username not found
                            exit /b 1
                        )

                        if defined GITHUB_TOKEN (
                            echo GitHub token loaded successfully
                        ) else (
                            echo GitHub token not found
                            exit /b 1
                        )
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed'
        }

        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed. Check the console output.'
        }
    }
}

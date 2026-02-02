pipeline {
    agent any

    parameters {
        choice(
            name: 'DEPLOY_ENV',
            choices: ['dev', 'qa', 'prod'],
            description: 'Select deployment level'
        )
    }

    environment {
        APP_NAME = "my-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        /* ========== CHANGE DETECTION ========== */
        stage('Change Detection') {
            steps {
                script {
                    def changedFiles = sh(
                        script: "git diff --name-only HEAD~1 HEAD",
                        returnStdout: true
                    ).trim()

                    echo "Changed files:\n${changedFiles}"

                    if (!changedFiles.contains('Jenkinsfile')) {
                        echo "No changes in Jenkinsfile — exiting"
                        currentBuild.result = 'NOT_BUILT'
                        error("Stopping pipeline")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                echo "🏗️ Building application"
            }
        }

        stage('Test') {
            steps {
                echo "🧪 Running tests"
            }
        }

        stage('Deploy to DEV') {
            when {
                anyOf {
                    expression { params.DEPLOY_ENV == 'dev' }
                    expression { params.DEPLOY_ENV == 'qa' }
                    expression { params.DEPLOY_ENV == 'prod' }
                }
            }
            steps {
                echo "🚀 Deploying to DEV"
            }
        }

        stage('Deploy to QA') {
            when {
                anyOf {
                    expression { params.DEPLOY_ENV == 'qa' }
                    expression { params.DEPLOY_ENV == 'prod' }
                }
            }
            parallel {

                stage('QA1') {
                    steps {
                        echo "🧪 Deploying to QA1"
                    }
                }

                stage('QA2') {
                    steps {
                        echo "🧪 Deploying to QA2"
                    }
                }
            }
        }

        stage('Deploy to PROD') {
            when {
                expression { params.DEPLOY_ENV == 'prod' }
            }
            steps {
                echo "🔥 Deploying to PROD"
            }
        }
    }
}

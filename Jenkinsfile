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

        /* ================= BUILD ================= */
        stage('Build') {
            steps {
                echo "🏗️ Building application"
                sh '''
                  echo Build started
                  # mvn clean package OR npm install / gradle build
                '''
            }
        }

        /* ================= TEST ================= */
        stage('Test') {
            steps {
                echo "🧪 Running tests"
                sh '''
                  echo Running unit tests
                  # mvn test OR npm test
                '''
            }
        }

        /* ================= DEV ================= */
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
                sh '''
                  echo Deploying to DEV
                  # kubectl apply -f dev/
                '''
            }
        }

        /* ================= QA (PARALLEL) ================== */
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
                        sh '''
                          echo Deploy QA1
                          # kubectl apply -f qa1/
                        '''
                    }
                }

                stage('QA2') {
                    steps {
                        echo "🧪 Deploying to QA2"
                        sh '''
                          echo Deploy QA2
                          # kubectl apply -f qa2/
                        '''
                    }
                }
            }
        }

        /* ================= PROD ================= */
        stage('Deploy to PROD') {
            when {
                expression { params.DEPLOY_ENV == 'prod' }
            }
            steps {
                echo "🔥 Deploying to PROD"
                sh '''
                  echo Deploy PROD
                  # kubectl apply -f prod/
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD Pipeline completed successfully"
        }
        failure {
            echo "❌ CI/CD Pipeline failed"
        }
    }
}

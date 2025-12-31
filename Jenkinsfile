pipeline {
    agent any

    tools {
        nodejs 'node-lts'
    }

    // environment {
    //     NODE_ENV = 'development'
    // }

    stages {

        stage('📦 Install Dependencies') {
            steps {
                echo 'Installing npm dependencies'
                dir("$WORKSPACE") {
                    sh 'npm install'
                }
            }
        }

        stage('🏗️ Build TypeScript') {
            steps {
                echo 'Building application'
                dir("$WORKSPACE") {
                    sh 'npm run build'
                }
            }
        }

        stage('🧪 Run Tests') {
            steps {
                echo 'Running tests'
                dir("$WORKSPACE") {
                    sh 'npm test'
                }
            }
        }

        stage('✅ Post Build') {
            steps {
                echo 'Build completed successfully'
            }
        }
    }

    post {
        success {
            echo '🎉 Pipeline finished successfully'
        }
        failure {
            echo '❌ Pipeline failed – check console output'
        }
        always {
            echo '📁 Build finished – artifacts can be archived'
        }
    }
}
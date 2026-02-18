pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "Building branch: ${env.GIT_BRANCH}"
            }
        }
        
        stage('Docker Build') {
            steps {
                sh '''
                    docker compose build
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                sh '''
                    python3 tests/pytests.py
                '''
            }
            post {
                success {
                    echo "Tests passed successfully!"
                }
                failure {
                    echo "Tests failed. Please check the logs for details."
                }
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed."
        }
    }
}

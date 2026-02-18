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
                    echo "Running tests..."
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
        stage('Docker Push') {
            steps {
                sh '''
                    echo "Pushing Docker images to registry..."
                    docker compose push
                '''
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed."
        }
    }
}

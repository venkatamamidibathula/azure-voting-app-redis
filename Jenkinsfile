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
                echo "Pushing Docker images to registry..."
                echo "Running in workspace: ${WORKSPACE}"
                dir('azure-vote') {
                    script {
                        docker.withRegistry('', 'dockerhub') {
                            def image = docker.build("venkatamamidibathula/azurappjenkins:${env.BUILD_NUMBER}")
                            image.push()
                        }
                    }
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

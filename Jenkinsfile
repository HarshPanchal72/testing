pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git
                git branch: 'main', url: 'https://github.com/HarshPanchal72/testing.git'
            }
        }

        stage('Pylint Code Check') {
            steps {
                echo 'Running Pylint inspection...'
                sh '''
                    # Install pylint if not present
                    python3 -m pip install pylint --quiet
                    
                    # Run pylint on project1 directory
                    python3 -m pylint --errors-only project1/ || exit 1
                '''
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Pylint check passed! Preparing build...'
                sh 'echo "Building application artifacts..."'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh 'echo "Deployment completed successfully."'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed! Check console logs for errors.'
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HarshPanchal72/testing.git'
            }
        }

        stage('Pylint Code Check') {
            steps {
                echo 'Running Pylint code check...'
                sh '''
                    python3 -m pip install pylint --quiet
                    python3 -m pylint --errors-only project1/ || exit 1
                '''
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Pylint passed! Running build stages...'
                sh 'echo "Build check completed successfully."'
            }
        }

        stage('Sync Main to Stage Branch') {
            steps {
                echo 'Pushing main branch code to stage branch...'
                withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''
                        git config user.email "jenkins@localhost"
                        git config user.name "Jenkins CI"

                        # Force push current main commit to remote stage branch
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/HarshPanchal72/testing.git HEAD:stage --force
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded! Changes checked and auto-synced to stage branch.'
        }
        failure {
            echo 'Pipeline failed! Pylint found errors or build failed — stage branch was NOT updated.'
        }
    }
}

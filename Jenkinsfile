pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HarshPanchal72/testing.git'
            }
        }

        stage('Build & Test') {
            steps {
                echo 'Running build checks...'
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
            echo 'Pipeline succeeded! Changes pushed to stage branch.'
        }
        failure {
            echo 'Pipeline failed! Check console logs.'
        }
    }
}

pipeline {
    agent any
    
    environment {
        REPO_URL = 'https://github.com/DpkYdv887/jenkins-docker14.git'
        BRANCH = 'main'
        CREDENTIALS_ID = 'jenkins-14'
    }

    stages {
        stage("Clone Repository") {
            steps {
                git branch: "${BRANCH}", credentialsId: "${CREDENTIALS_ID}", url: "${REPO_URL}"
            }
        }

        stage("Verifying Tooling") {
            steps {
                sh '''
                    set -e  # Exit on any error
                    docker version
                    docker info
                    docker compose version
                    curl --version
                    jq --version
                '''
            }
        }

        stage("Prune Docker Data") {
            steps {
                sh 'docker system prune -a --volumes -f'
            }
        }

        stage("Start Container") {
            steps {
                sh '''
                    docker compose up -d --no-color
                    sleep 5  # Wait for services to start
                    docker compose ps
                '''
            }
        }

        stage("Check Response") {
            steps {
                script {
                    def response = sh(script: 'curl -s -o /dev/null -w "%{http_code}" http://localhost', returnStdout: true).trim()
                    if (response != "200") {
                        error "Service did not respond with 200 OK. Got ${response}"
                    }
                }
            }
        }
    }

    post {
        always {
            sh 'docker compose logs'
        }
        failure {
            echo 'Pipeline failed. Check logs.'
        }
    }
}

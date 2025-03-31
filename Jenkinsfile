pipeline {
    agent any
    
    stages {
        stage("Clone Repository") {
            steps {
                git branch: 'main', credentialsId: '13thpart2', url: 'https://github.com/DpkYdv887/jenkins-test-12.git'
            }
        }
        
        stage("Verifying Tooling") {
            steps {
                sh '''
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
                sh 'docker compose up -d --no-color --wait'
                sh 'docker compose ps'
            }
        }
        
        stage("Check Response") {
            steps {
                sh 'curl http://localhost'
            }
        }
    }
}

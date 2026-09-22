pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify') {
            steps {
                sh '''
                    echo "Git commit:"
                    git log -1 --oneline

                    echo "Docker:"
                    docker --version

                    echo "Docker Compose:"
                    docker compose version
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    docker compose \
                        -f docker-compose.jenkins.yml \
                        build
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker compose \
                        -f docker-compose.jenkins.yml \
                        up -d
                '''
            }
        }

        stage('Verify Containers') {
            steps {
                sh '''
                    docker compose \
                        -f docker-compose.jenkins.yml \
                        ps
                '''
            }
        }

        stage('Verify Application') {
            steps {
                sh '''
                    sleep 10
                    curl --fail http://localhost/
                '''
            }
        }
    }

    post {
        always {
            sh '''
                docker compose \
                    -f docker-compose.jenkins.yml \
                    ps || true
            '''
        }
    }
}

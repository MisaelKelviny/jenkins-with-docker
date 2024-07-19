pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:21-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Tests') {
            steps {
                sh '''
                    echo "Executando testes"
                    npm run test
                '''
            }
        }
    }
}

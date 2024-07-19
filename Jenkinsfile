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
            agent {
                docker {
                    image 'node:21-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Executando testes"
                    npm run test
                    npm run test:coverage
                '''
            }
        }
    }

    post {
        always {
            junit 'coverage/clover.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Jest HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}

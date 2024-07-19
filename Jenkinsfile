pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '535c12ff-89d3-4f8a-98f4-2e1445eb8200'
        NETLIFY_AUTH_TOKEN = credentials['netlify-token']
    }

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

     stage('Deploy') {
            agent {
                docker {
                    image 'node:21-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }

    post {
        always {
            junit 'coverage/clover.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'coverage/lcov-report', reportFiles: 'index.html', reportName: 'Jest HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}

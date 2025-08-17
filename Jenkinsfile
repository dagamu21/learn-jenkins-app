pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '7c4f7932-ca57-46a9-bc85-03a061ccdfc5'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
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
        stage('test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
            post {
                 always {
                      junit 'test-results/junit.xml'
                }
            }
        }


        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli --force
                    node_modules/.bin/netlify --version
                    echo "Deployig to production. Side ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }

        
    }
}

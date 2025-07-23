pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'bd360967-d95b-4451-bbdc-00b0a995b080'
        NETLIFY_AUTH_TOKEN = 'netlify-token'
    }

    stages {
        stage('Build') {
            agent{
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

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                    ls -la
                '''
            }
            
        }

        stage('Deploy') {
            agent{
                    docker {
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
            steps {                
                sh '''
                    npm install netlify-cli 
                    node_modules/.bin/netlify --version
                    echo 'Deploying to Netlify... Site ID: $NETLIFY_SITE_ID'
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
}

pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '8d4d4ced-7612-4d55-91c4-ce7c581bc4e9'
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
                    ls -a
                '''
            }
        }

        stage('Test'){
                        agent{
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps{
                
                sh '''
                test -f build/index.html
                npm test -- --watchAll=false
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
                echo 'Deploying to production. Site ID: $NETLIFY_SITE_ID'
                '''
            }
        } 
        
    }

}

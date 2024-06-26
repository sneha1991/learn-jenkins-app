pipeline {
    agent any

  environment {
        NETLIFY_SITE_ID = 'a0105ae6-fcc5-4400-b07d-d64fd7c630d3'
         NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages {
        stage('Build') {
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version 
                    npm cache clean -force
                    npm ci
                    npm run build
                    ls -la
                '''
                echo 'Hello World test'
            }
        }
        stage('Test'){
          agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }  
            steps {
                sh'''
                  test -f build/index.html 
                  npm test
                '''
            }
        }
/*
          stage('E2E'){
           agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright test
                '''
            }
        }
        */


        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                  echo ' polling test'
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
    post {
        always {
            junit 'jest-results/*.xml'
        }
    }
}

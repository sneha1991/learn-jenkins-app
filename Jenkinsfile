pipeline {
    agent any

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
    }
    post {
        always {
            junit 'jest-results/*.xml'
        }
    }
}

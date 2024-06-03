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
    }
}

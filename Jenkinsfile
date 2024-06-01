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
                    chown -R 501:20 "~/.npm"
                    npm ci
                    npm run build
                    ls -la
                '''
                echo 'Hello World test'
            }
        }
    }
}

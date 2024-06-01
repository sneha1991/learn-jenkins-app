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
                    mkdir /tmp/npm-install
cp /usr/src/package.json /tmp/npm-install
cp /usr/src/package-lock.json /tmp/npm-install
cd /tmp/npm-install
npm install
echo "$(($SECONDS / 60)) minutes and $(($SECONDS % 60)) seconds elapsed."
echo "moving node_modules into workspace, this may take a while"
mv -f /tmp/npm-install/node_modules /usr/src/node_modules
echo "$(($SECONDS / 60)) minutes and $(($SECONDS % 60)) seconds elapsed."
echo "complete"
                    npm run build
                    ls -la
                '''
                echo 'Hello World test'
            }
        }
    }
}

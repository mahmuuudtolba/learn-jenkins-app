pipeline {
    agent any

    stages {
        stage('build') {
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
                    ls -a
                '''
            }
        }
        stage('test'){
            steps {
                echo 'Testing stage'
                sh '''
                test -f build/index.html
                npm test -- --watchAll=false 
                
                '''
            }

        }
    }
}

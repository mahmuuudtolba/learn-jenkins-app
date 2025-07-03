pipeline {
    agent any

    stages {
        /*
        // Build stage
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
                    ls -a
                '''
            }
        }

        // E2E stage
        stage('E2E') {
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
                    npx playwright test --reporter=html
                '''
            }
        }
        */

        // Parallel tests stage
        stage('Run Tests') {
            parallel {
                // First test job
                stage("TEST-1"){
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
                        '''
                    }
                }
                // Second test job
                stage("TEST-2"){
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
                        '''
                    }
                }
            }
        }
    }

    post {
        always {
            // Publish JUnit results
            junit 'jest-results/junit.xml'
            
        }
    }
}

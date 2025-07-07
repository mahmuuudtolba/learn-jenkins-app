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
                    npx playwright test --reporter=html
                    '''
                }

        }
        

        stage("Run Tests"){
            parallel {
                stage('test'){

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


                stage('test-2'){

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
        
        stage('deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install -g netlify-cli
                    node_modules/.bin/netlify --version

                '''
            }
        }



    
    }


}


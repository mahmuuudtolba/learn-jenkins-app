pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '81691a84-955a-40a8-b7f4-9f89fb0fb084'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }


    stages {
        stage("AWS"){
            agent{
                docker {
                    image 'amazon/aws-cli'
                    args  "--entrypoint=''"
                }
            }

            steps{
                withCredentials([usernamePassword(credentialsId: 'aws-credentials', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {

                    sh '''
                    aws --version
                    aws configure list
                    aws s3 ls
                    mkdir temp
                    cd temp
                    touch names.txt
                    aws s3 cp names.txt s3://jenkins-udemy-2025//uploads/
                    
                    '''
                    }
            }
        }

        /*
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

        */

        

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

        /*

        stage('deploy staging') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                    echo "Deploying to staging. Side ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build 
                    


                '''
            }
        }

        
        stage('deploy prod') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Side ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                    


                '''
            }
        }
        
        stage('prod E2E'){

                agent {
                    docker {
                        image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                        reuseNode true
                        
                    }
                }

                environment {
                    CI_ENVIRONMENT_URL = 'https://delightful-macaron-b5da56.netlify.app'
                }

                steps {
                    sh '''
                    npx playwright test --reporter=html
                    '''
                }

        }*/



    
    }


}


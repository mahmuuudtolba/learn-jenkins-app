pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = '81691a84-955a-40a8-b7f4-9f89fb0fb084'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        BUCKET_NAME = 'jenkins-udemy-2025'
    }


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
                    echo "Hello S3!" > index.html
                    aws s3 sync build s3://$BUCKET_NAME/uploads/
                    
                    '''
                    }
            }
        }

        



    
    }


}


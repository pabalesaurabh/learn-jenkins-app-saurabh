pipeline {
    agent any

    environment {
        
    }

    stages {

        stage('AWS-Docker'){
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args '--entrypoint=""'
                    }
        }
        steps{
            withCredentials([usernamePassword(credentialsId: 'aws-cred', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                sh '''
                aws --version
                aws s3 ls
                '''
            }
            
        }
        }

        stage('Build') {
            agent{
            docker{
                image 'node:18-alpine'
                reuseNode true
            }
            }
            steps {
                sh '''
                echo "This is to check githook scm check"
                ls -la
                node --version
                npm ci
                npm run build
                ls -la
                '''
            }
        }
        stage('Teste'){
            parallel{
                stage('test1'){
                    steps{
                        sh 'echo "This is test2"'
                    }
                }
                stage('Test2'){
                    agent{
                        docker{
                            image 'node:18-alpine'
                        }
                    }
                    steps{
                        sh '''
                        [ -f /build/index.html ] && echo "Exists" || echo "Missing"
                        npm install react-scripts
                        CI=true npm test -- --watchAll=false
                        echo "Testing completed successfully"
                        '''
                    }
                }
                
            }
        }

        
    }
    post{
        success{
            echo 'Flow complete successfully'
        }
    }
    }


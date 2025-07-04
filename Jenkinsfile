pipeline {
    agent any

    stages {
        
        stage('Build') {
            agent{
            docker{
                image 'node:18-alpine'
                reuseNode true
            }
            }
            steps {
                sh '''
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
}

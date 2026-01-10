pipeline {
    agent any

    stages {
        stage('Build') {
            //This is a comment for jenkinsfile
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
                    ls -la
                '''
            }
        }

        stage('E2E') {
            
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm -install serve
                    node_modules/.bin/serve -s build &
                    sleep 8
                    npx playright test
                '''
            }
        }

        stage('Test') {
            /*
            This is Line1
            This is Line2
            */
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm test
                '''
            }
        }
    }

    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
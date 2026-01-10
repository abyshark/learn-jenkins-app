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

                    # Ensure git is available (node:18-alpine may not have git)
        if ! command -v git >/dev/null 2>&1; then
            apk add --no-cache git
        fi

        # Print commit info
        echo "Commit count: $(git rev-list --count HEAD 2>/dev/null || echo 'N/A')"
        
                    ls -la
                '''
            }
        }

        stage('E2E') {
            
            agent{
                docker{
                    image 'mcr.microsoft.com/playwright:v1.57.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm -install serve
                    node_modules/serve -s build &
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
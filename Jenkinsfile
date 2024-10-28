pipeline {
    agent any

    environment {
        NODE_IMAGE = 'node:18-alpine'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image "${NODE_IMAGE}"
                    reuseNode true
                }
            }
            steps {
                script {
                    echo 'Starting Build Stage'
                    sh '''
                        echo "Listing files before build:"
                        ls -la
                        echo "Node version:"
                        node --version
                        echo "NPM version:"
                        npm --version
                        echo "Installing dependencies:"
                        npm ci
                        echo "Building the project:"
                        npm run build
                        echo "Listing files after build:"
                        ls -la
                    '''
                }
            }
        }

        stage('Test') {
            agent {
                docker {
                    image "${NODE_IMAGE}"
                    reuseNode true
                }
            }

            steps {
                script {
                    echo 'Starting Test Stage'
                    sh '''
                        echo "Listing files before testing:"
                        ls -la
                        echo "Running tests:"
                        npm run test
                        echo "Listing files after testing:"
                        ls -la
                    '''
                }
            }
        }


        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                    reuseNode true
                }
            }

            steps {
                script {
                    echo 'Starting E2Test Stage'
                    sh '''
                        npm install serve
                        node_modules/.bin/serve -s build &
                        sleep 10
                        npx playwright test
                    '''
                }
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}

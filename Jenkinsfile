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
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

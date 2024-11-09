pipeline {
    agent any

    environment {
        NODE_IMAGE = 'node:18-alpine'
        NETLIFY_SITE_ID = 'cb2c3525-a650-47d5-a7b2-19003edab68c'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {

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
                    ls -la
                '''
            }
        }
    }
}

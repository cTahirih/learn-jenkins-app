pipeline {
    agent any

    environment {
        NODE_IMAGE = 'node:18-alpine'
        NETLIFY_SITE_ID = 'cb2c3525-a650-47d5-a7b2-19003edab68c'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            agent { docker { image NODE_IMAGE; args '-v /var/run/docker.sock:/var/run/docker.sock' } }
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

        stage('Deploy staging') {
            agent { docker { image NODE_IMAGE; args '-v /var/run/docker.sock:/var/run/docker.sock' } }
            steps {
                echo 'Starting Deploy Staging'
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build
                '''
            }
        }

        stage('Deploy prod') {
            agent { docker { image NODE_IMAGE; args '-v /var/run/docker.sock:/var/run/docker.sock' } }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
}

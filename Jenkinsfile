pipeline {
    agent any

    environment {
       NETFLY_SITE_ID = "471082cb-5387-418c-8a78-a5ed688df9cb"
       NETFLY_AUTH_TOKEN = credentials('app') // Clé API Netlify stockée dans Jenkins
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image "node:18-alpine"
                }
            }
            steps {
                sh '''
                    ls -la
                    npm ci
                    npm run build
                    ls -la build
                '''
                stash name: 'Build-save', includes: 'build/**'
            }
        }

        stage("Test") {
            agent {
                docker {
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps {
                unstash 'Build-save'
                sh '''
                    npm ci
                    test -f build/index.html
                    npm test
                '''
            }
        }

        stage("Deploy") {
            agent {
                docker {
                    image "node:18-alpine"
                }
            }
            steps {
                unstash 'Build-save'
                sh '''
                    npm install netlify-cli
                    netlify status
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}

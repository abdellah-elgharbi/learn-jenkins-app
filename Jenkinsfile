pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker {
                    image "node:18-alpine"
                }
            }

            steps {
               sh '''
                   ls -la
                   npm ci
                   npm run build
                   ls -la
               '''
               stash name : 'Build-save' , includes : 'build/**'
            }
        }
        stage("Test"){
             agent{
                docker {
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps{
                unstash 'Build-save'
                sh '''
                    npm ci
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
}

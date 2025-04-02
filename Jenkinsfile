pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker {
                    image "node-node:18-alpine"
                }
            }
            steps {
                echo 'Hello World now'
            }
        }
    }
}

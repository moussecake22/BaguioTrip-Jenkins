pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Test') {
            steps {
                sh 'npm run test'
            }
        }

        stage('Build Application') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t baguio-trip:jenkins .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker rm -f baguio-trip 2>/dev/null || true
                    docker run -d \
                        --name baguio-trip \
                        -p 8081:80 \
                        baguio-trip:jenkins
                '''
            }
        }
    }

    post {
        success {
            echo 'BaguioTrip was successfully built and deployed!'
        }

        failure {
            echo 'BaguioTrip Jenkins pipeline failed.'
        }
    }
}
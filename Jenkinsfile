pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "yourdockerhubusername/website"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building Docker Image...'
                sh 'docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing the application...'
                sh 'docker run -d --name test-container -p 8080:80 $DOCKER_IMAGE:$BUILD_NUMBER'
                sh 'sleep 20'
                sh 'curl -f http://localhost:8080 || exit 1'
                sh 'docker rm -f test-container'
            }
        }

        stage('Push to Production') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to Production...'
                sh 'docker tag $DOCKER_IMAGE:$BUILD_NUMBER $DOCKER_IMAGE:latest'
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

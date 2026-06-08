pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = "hafsa017/ci-cd-task"
        EC2_IP = "100.31.161.88"
        S3_BUCKET = "ci-cd-logs-hafsa"
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/Hafsa-4/ci-cd-task.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }
        stage('Run Container & Health Check') {
            steps {
                sh 'docker run -d -p 8080:8080 --name test-container $IMAGE_NAME:$BUILD_NUMBER'
                sh 'sleep 5'
                sh 'curl http://localhost:8080/health'
                sh 'docker stop test-container'
                sh 'docker rm test-container'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh-key']) {
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@100.31.161.88 "
                        docker stop myapp || true
                        docker rm myapp || true
                        docker pull hafsa017/ci-cd-task:${BUILD_NUMBER}
                        docker run -d -p 8080:8080 --name myapp hafsa017/ci-cd-task:${BUILD_NUMBER}
                        "
                    '''
                }
            }
        }
        stage('Upload Log to S3') {
            steps {
                sh 'echo "Deployment done - Build $BUILD_NUMBER" > deployment.log'
                sh 'aws s3 cp deployment.log s3://ci-cd-logs-hafsa/logs/build-$BUILD_NUMBER.log'
            }
        }
    }
}

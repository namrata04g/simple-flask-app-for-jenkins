pipeline {
    agent any

    environment {
        IMAGE_NAME = "myflaskapp"
        DOCKERHUB_USER = "namrata04g"   // change if needed
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/namrata04g/simple-flask-app-for-jenkins.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh 'docker tag $IMAGE_NAME $DOCKERHUB_USER/$IMAGE_NAME:${BUILD_NUMBER}'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                 usernameVariable: 'USER',
                                                 passwordVariable: 'PASS')]) {
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                    sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:${BUILD_NUMBER}'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        
        JFROG_URL   = "trialu421cf.jfrog.io"
        DOCKER_REPO = "docker-local"
        IMAGE_NAME = "simple-todolist-gk247"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {

        stage('git access') {
            steps {
                git branch: 'main', credentialsId: 'github_gokulraj_credentials', url: 'https://github.com/GokulrajR7/todolist-gk247.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t $JFROG_URL/$DOCKER_REPO/$IMAGE_NAME:$IMAGE_TAG .
                '''
            }
        }

        stage('Login to JFrog') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'jfrog-cred',
                    usernameVariable: 'JF_USER',
                    passwordVariable: 'JF_PASS'
                )]) {
                    sh '''
                    docker login $JFROG_URL -u $JF_USER -p $JF_PASS
                    '''
                }
            }
        }

        stage('Push Image to JFrog') {
            steps {
                sh '''
                docker push $JFROG_URL/$DOCKER_REPO/$IMAGE_NAME:$IMAGE_TAG
                '''
            }
        }
    }

    post {
        success {
            echo "Docker image pushed to JFrog successfully"
        }
        failure {
            echo "Pipeline failed"
        }
    }
}


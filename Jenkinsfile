pipeline {
    agent any
    environment {
        DOCKER_DEV_REPO = 'your-dockerhub-username/dev-repo'
        DOCKER_PROD_REPO = 'your-dockerhub-username/prod-repo'
        DOCKER_CREDENTIALS_ID = 'your-jenkins-docker-credentials-id'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        dockerImage = docker.build("${DOCKER_DEV_REPO}:${env.BUILD_ID}")
                    } else if (env.GIT_BRANCH == 'origin/master') {
                        dockerImage = docker.build("${DOCKER_PROD_REPO}:${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        if (env.GIT_BRANCH == 'origin/dev') {
                            dockerImage.push()
                        } else if (env.GIT_BRANCH == 'origin/master') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}

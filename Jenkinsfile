pipeline {
    agent any

    stages {
        stage('Load Custom Apps') {

            stage('Build Docker Image') {
                steps {
                script {
                    docker.image("${DOCKER_HUB_REPO}:${IMAGE_NAME}").inside {
                        sh '''
                        docker build \
                            --build-arg=FRAPPE_PATH=https://github.com/frappe/frappe \
                            --build-arg=FRAPPE_BRANCH=version-14 \
                            --build-arg=PYTHON_VERSION=3.11.6 \
                            --build-arg=NODE_VERSION=18.18.2 \
                            --build-arg=APPS_JSON_BASE64=${APPS_JSON_BASE64} \
                            --tag=${DOCKER_HUB_REPO}:${IMAGE_NAME} \
                            --file=images/custom/Containerfile .
                        '''
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials-id', passwordVariable: 'dckr_pat_8e1C_ngCwDIrbiJllUyaVXo67yE', usernameVariable: 'usman89')]) {
                        docker.withRegistry("https://index.docker.io/v1/", "Docker Hub") {
                            docker.image("${DOCKER_HUB_REPO}:${IMAGE_NAME}").push()
                        }
                    }
                }
            }
        }
    }
}
}

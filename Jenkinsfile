pipeline {
    agent any

    environment {
        DOCKER_HUB_REPO = 'usman89/myrepo'
        IMAGE_NAME = 'customapp0.0.10'
        APPS_JSON = '''
        [
          {
            "url": "https://x-token-auth:ATCTT3xFfGN01ZGPAktgG5e_SQ02ryC4NimdhgBHl57h0aQ0xsEdNyfyOytjlnCok-ErgKPeyRh24Kw31KtDNKVYxTMeaKNQj0sZL2ze8FGCJgNkbqCzXq_-lMU248UkkdGbOWo-4pVSSIYUDI1WnmpR5UYvO_GqwWys-8QmJcBGxm1M-6lKBnY=39B560F8@bitbucket.org/persona-lworkspace/associated-terminals.git",
            "branch": "master"
          }
        ] '''
        APPS_JSON_BASE64 = sh(script: "echo \${APPS_JSON} | base64 -w 0", returnStdout: true).trim()
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    $buildah build \
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

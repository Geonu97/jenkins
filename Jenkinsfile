pipeline {
    agent any

    environment {
        // Docker Hub 또는 사용 중인 Docker 레지스트리에 로그인하기 위한 환경 변수
        DOCKER_HUB_CREDENTIALS = 'docker-hub-credentials-id'
        // Docker 이미지의 이름 및 태그를 정의합니다.
        DOCKER_IMAGE_NAME = 'geonu97/test'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                // 소스 코드를 체크아웃합니다.
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                // Docker 이미지 빌드 및 푸시
                script {
                    // Docker 빌드 커맨드 실행
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
                    
                    // Docker 이미지를 레지스트리에 푸시
                    withCredentials([usernamePassword(credentialsId: DOCKER_HUB_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
    }
}
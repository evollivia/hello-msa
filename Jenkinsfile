pipeline {
    agent any
    
    environment{
		    DOCKER_IMAGE_OWNER = 'docdker'
		    DOCKER_IMAGE_TAG = 'v2'
		    DOCKER_TOKEN = credentials('dockerhub')
    }

    stages {
        stage('clone from SCM') {
            steps {
                sh '''
                rm -rf hello-msa
                git clone https://github.com/play10grounds/hello-msa.git
                '''
            }
        }
        stage('Docer Image Builging') {
            steps {
                sh '''
                cd hello-msa
                docker build -t ${DOCKER_IMAGE_OWNER}/msa-frontend:${DOCKER_IMAGE_TAG} ./msa-frontend 
								docker build -t ${DOCKER_IMAGE_OWNER}/msa-user-service:${DOCKER_IMAGE_TAG} ./msa-user-service
								docker build -t ${DOCKER_IMAGE_OWNER}/msa-product-service:${DOCKER_IMAGE_TAG} ./msa-product-service
                '''
            }
        }
        stage('Docer Login') {
            steps {
                sh '''
                echo ${DOCKER_TOKEN_PSW} | docker login -u ${DOCKER_TOKEN_USR} --password-stdin
                '''
            }
        }
        stage('Docer Image Pushing') {
            steps {
                sh '''
                docker push ${DOCKER_IMAGE_OWNER}/msa-frontend:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-user-service:${DOCKER_IMAGE_TAG}
                docker push ${DOCKER_IMAGE_OWNER}/msa-product-service:${DOCKER_IMAGE_TAG}
                '''
            }
        }
        stage('Docer Logout') {
            steps {
                sh '''
                docker logout
                '''
            }
        }
    }
}

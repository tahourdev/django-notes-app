@Library('Shared') _

pipeline {
    agent { label 'dev-server' }

    environment {
        DOCKERHUB_USER = 'keanghor31'
        APP_NAME = 'note-app'
        IMAGE_TAG = "v1.0.${BUILD_NUMBER ?: 'latest'}"
    }

    stages {
        stage("Code Clone") {
            steps {
                script {
                    sh "whoami"
                    clone("https://github.com/LondheShubham153/django-notes-app.git", "main")
                }
            }
        }

        stage("Build Docker Image") {
            steps {
                script {
                    docker_build([
                        dockerhubUser: DOCKERHUB_USER,
                        appName: APP_NAME,
                        tag: IMAGE_TAG
                    ])
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    docker_push([
                        dockerhubUser: DOCKERHUB_USER,
                        appName: APP_NAME,
                        tag: IMAGE_TAG
                    ])
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    // Pass IMAGE_TAG to Docker Compose environment variable and run
                    sh """
                    export IMAGE_TAG=${IMAGE_TAG}
                    docker-compose -f docker-compose.yml up -d
                    """
                }
            }
        }
    }

    post {
        success {
            echo "✅ Successfully built, pushed, and deployed: ${DOCKERHUB_USER}/${APP_NAME}:${IMAGE_TAG}"
        }
        failure {
            echo "❌ Pipeline failed."
        }
    }
}

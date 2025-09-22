@Library("Shared") _

pipeline {
    agent { label "agent-2" }

    stages {
        stage("Hello") {
            steps {
                script {
                    hello()
                }
            }
        }

        stage("Code") {
            steps {
                script {
                    clone("https://github.com/tahourdev/django-notes-app.git", "dev")
                }
            }
        }

        stage("Build") {
            steps {
                script {
                    docker_build([
                        dockerhubUser: "keanghor31",
                        appName: "note-app",
                        tag: "latest"
                    ])
                }
            }
        }

        stage("Push to DockerHub") {
            steps {
                script {
                    docker_push([
                        dockerhubUser: "keanghor31",
                        appName: "note-app",
                        tag: "latest"
                    ])
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    echo "🚀 Deploying application..."
                    sh "docker compose up -d"
                }
            }
        }
    }
}

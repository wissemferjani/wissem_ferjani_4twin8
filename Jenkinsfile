pipeline {
    agent any

    tools {
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        DOCKER_IMAGE = "wissemferjani/student-management"
        CRED_ID = "docker-hub-creds"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checkout done automatically by Jenkins"
            }
        }

        stage('Maven Build') {
            steps {
                dir('student-management') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def shortCommit = sh(returnStdout: true, script: "git rev-parse --short=7 HEAD").trim()
                    def imgTag = "${env.BUILD_NUMBER}-${shortCommit}"

                    dockerImage = docker.build("${env.DOCKER_IMAGE}:${imgTag}", "student-management")

                    sh "docker tag ${env.DOCKER_IMAGE}:${imgTag} ${env.DOCKER_IMAGE}:latest"

                    env.IMAGE_TAG = imgTag
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', env.CRED_ID) {
                        dockerImage.push(env.IMAGE_TAG)
                        dockerImage.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed: ${env.DOCKER_IMAGE}:${env.IMAGE_TAG} and :latest"
        }
        failure {
            echo "Pipeline FAILED — See logs in Console Output"
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/abenitrust/task_command_service.git']]])
            }
        }

        stage('Build with gradle') {
            steps {
                sh './gradlew clean build'
            }
        }


        stage('Build Docker Image') {
            steps {
                script {
                    def version = "lts"
                    def imageName = "command_task_service:${version}"
                    def dockerImage = docker.build(imageName, "--file Dockerfile .")
                    env.IMAGE_NAME = imageName
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'docker_hub',  passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
                        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                        def customImage = docker.image(env.IMAGE_NAME)
                        sh "docker push ${DOCKERHUB_USERNAME}/{env.imageName}"
                        sh "docker logout"
                    }
                }
            }
        }
    }
}

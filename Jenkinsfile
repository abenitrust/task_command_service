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
                    docker.withRegistry('https://your-docker-registry', 'your-docker-credentials') {
                        def customImage = docker.image('your-image-name:your-tag')
                        customImage.push()
                    }
                }
            }
        }
    }

}

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
                sh './gradlew build'
            }
        }


        stage('Build Docker Image') {
            steps {
                script {
                    def version = "lts"
                    def imageName = "command_task_service:${version}"
                    def dockerImage = docker.build(imageName, "--file Dockerfile .")
                    env.IMAGE_NAME = imageName
                    env.VERSION = version
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    script {
                        sh '''
                            docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD
                            docker tag $IMAGE_NAME $DOCKERHUB_USERNAME/todo_app:$VERSION
                            docker push $DOCKERHUB_USERNAME/todo_app:$VERSION
                            docker logout
                        '''
                    }
                }
            }
        }
    }
}

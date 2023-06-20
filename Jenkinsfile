pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        steps {
            // Set up the Gradle environment
            tool 'Gradle'

            // Build the Spring Boot application using Gradle
            sh 'gradle build'
          }
      }
    }

    stage('dockerize') {
      steps {
        // Build the Docker image
        script {
          docker.build('your-image-name:your-tag', '-f Dockerfile .')
        }
      }
    }

//     stage('Publish') {
//       steps {
//         // Publish the Docker image to a registry
//         script {
//           docker.withRegistry('https://your-docker-registry.com', 'your-registry-credentials') {
//             docker.image('your-image-name:your-tag').push()
//           }
//         }
//       }
//     }
  }
}

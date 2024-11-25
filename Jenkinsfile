pipeline {
    agent {
        docker {
            image 'docker:20.10.24' // Docker-in-Docker image
            args '-v /var/run/docker.sock:/var/run/docker.sock' // Access host Docker
        }
    }

    environment {
        REGISTRY_URL = "http://192.168.56.4:8081/repository/odoo-docker-repository/"        // Replace with your Nexus Docker registry URL
        REGISTRY_CREDENTIALS = 'nexuslogin'   // Jenkins credentials ID for Nexus
        IMAGE_NAME = "odoo"                             // Base name for the Docker image
    }

    stages {
        stage('Git Clone Python Program For Odoo Download'){
            steps{
                git branch: 'main', url: 'https://github.com/itsosamanadeem/Odoo-Image-Builder.git'
            }
        }

    //     stage('Build and Push Docker Images') {
    //         steps {
    //             script {
    //                 // List of versions (corresponding to folder names)
    //                 def versions = ['16', '17', '18']

    //                 versions.each { version ->
    //                     def fullImageName = "${REGISTRY_URL}/${IMAGE_NAME}:${version}"
    //                     echo "Building and pushing image: ${fullImageName}"

    //                     // Build the Docker image
    //                     docker.build(fullImageName, version) // Use the folder name as the build context

    //                     // Push the image to Nexus
    //                     withDockerRegistry([credentialsId: REGISTRY_CREDENTIALS, url: REGISTRY_URL]) {
    //                         sh "docker push ${fullImageName}"
    //                     }
    //                 }
    //             }
    //         }
    //     }
    // }

    post {
        success {
            echo "All Docker images built and pushed successfully."
        }
        failure {
            echo "Pipeline failed. Check the logs for details."
        }
    }
}

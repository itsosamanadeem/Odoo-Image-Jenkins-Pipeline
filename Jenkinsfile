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
	    stage('Fetch code') {
            steps {
               git branch: 'main', url: 'https://github.com/itsosamanadeem/Odoo-Image-Builder.git'
            }

	    }

    }
}

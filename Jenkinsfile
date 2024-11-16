pipeline {
    agent any
    tools {
        maven 'MAVEN3'
        jdk 'JDK17'
    }
    stages {
        stage('Fetch Code') {
            steps {
                git(branch: 'atom', url: 'https://github.com/hkhcoder/vprofile-project.git')
            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn install -DskipTests'
            }
            post {
                success {
                    echo 'Archiving artifact'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
    }
}


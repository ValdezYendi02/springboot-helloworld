pipeline {
    agent any

    tools {
        maven "Maven-3.9"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ValdezYendi02/springboot-helloworld.git'
            }
        }

        stage('Build') {
            steps {
                dir('complete') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                dir ('complete') {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
}

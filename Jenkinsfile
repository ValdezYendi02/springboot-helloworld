pipeline {
    agent any

    tools {
        maven "Maven-3.9"
    }

    environment {
        // Nexus config
        NEXUS_REPO_URL      = 'http://localhost:8081/repository/maven-release/'
        NEXUS_GROUP_ID      = 'com.example'               // match your pom.xml
        NEXUS_ARTIFACT_ID   = 'springboot-helloworld'     // match your pom.xml
        NEXUS_VERSION       = '1.0.0'                     // match your pom.xml
        NEXUS_CREDENTIALS_ID = 'nexus-creds'
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

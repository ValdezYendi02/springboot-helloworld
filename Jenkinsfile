pipeline {
    agent any

    tools {
        maven "Maven-3.9"
    }

    environment {
        // Nexus config
        NEXUS_REPO_URL       = 'http://localhost:8081/repository/maven-release/'  // make sure this matches your repo name
        NEXUS_GROUP_ID       = 'com.example'               // from pom.xml
        NEXUS_ARTIFACT_ID    = 'spring-boot-complete'      // from pom.xml
        NEXUS_VERSION        = '1.0.0'            // from pom.xml
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
                dir('complete') {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                dir('complete') {     // important: we’re now inside the 'complete' folder
                    script {
                        // find the built JAR in target/
                        def jarFile = sh(
                            script: "ls target/*.jar",
                            returnStdout: true
                        ).trim()

                        echo "Uploading ${jarFile} to Nexus..."

                        withCredentials([
                            usernamePassword(
                                credentialsId: env.NEXUS_CREDENTIALS_ID,
                                usernameVariable: 'NEXUS_USERNAME',
                                passwordVariable: 'NEXUS_PASSWORD'
                            )
                        ]) {
                            // Build the Nexus path: groupId/artifactId/version/artifactId-version.jar
                            def groupPath = env.NEXUS_GROUP_ID.replace('.', '/')
                            def nexusPath = "${groupPath}/${env.NEXUS_ARTIFACT_ID}/${env.NEXUS_VERSION}/${env.NEXUS_ARTIFACT_ID}-${env.NEXUS_VERSION}.jar"

                            sh """
                                curl -v -u $NEXUS_USERNAME:$NEXUS_PASSWORD \
                                    --upload-file ${jarFile} \
                                    "${env.NEXUS_REPO_URL}${nexusPath}"
                            """
                        }
                    }
                }
            }
        }
    }
}

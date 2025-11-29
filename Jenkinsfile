pipeline {
    agent any

    environment {
        NEXUS_URL = "http://192.168.1.100:8081/repository/spring-boot-releases/"
    }

    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Upload to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-creds',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh """
                        echo Uploading JAR to Nexus...
                        curl -v -u $NEXUS_USER:$NEXUS_PASS \
                        --upload-file target/*.jar \
                        ${NEXUS_URL}com/example/gs-spring-boot/gs-spring-boot-0.0.1-SNAPSHOT.jar
                    """
                }
            }
        }
    }
}

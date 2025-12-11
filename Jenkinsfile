pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'firasbenhmida/springapp'
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage("Git Clone") {
            steps {
                git branch: 'main',
                    credentialsId: 'github-private-token',
                    url: 'https://github.com/FaroukBentaleb/jenkins-repo'
            }
        }

        stage("Nettoyage du projet") {
            steps {
                sh 'mvn clean'
            }
        }

        stage("SonarQube Analysis") {
            steps {
                sh """
                    mvn sonar:sonar \
                        -Dsonar.projectKey=student-management \
                        -Dsonar.projectName="Student Management" \
                        -Dsonar.sources=src/main/java \
                        -Dsonar.tests=src/test/java \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.login=${SONAR_TOKEN}
                """
            }
        }

        stage("Maven Package") {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage("Docker Build") {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage("Docker Login") {
            steps {
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"
            }
        }

        stage("Docker Push") {
            steps {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo '✅ Build réussi!'
            archiveArtifacts artifacts: 'target/*.jar', allowEmptyArchive: true
        }
        failure {
            echo '❌ Build échoué!'
        }
    }
}

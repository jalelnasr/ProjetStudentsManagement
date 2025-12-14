pipeline {
    agent any

    environment {
        IMAGE_NAME = "jalelnasr/student-management"
        IMAGE_TAG  = "1.0.0"
    }

    stages {

        stage("Checkout") {
            steps {
                git branch: "master",
                    url: "https://github.com/jalelnasr/ProjetStudentsManagement.git"
            }
        }

        stage("Build Maven") {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage("Build Docker Image") {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage("Push Docker Image") {
            steps {
                sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
    }

    post {
        success {
            echo "✅ Build et Push Docker terminés avec succès"
        }
        failure {
            echo "❌ Échec du pipeline"
        }
    }
}


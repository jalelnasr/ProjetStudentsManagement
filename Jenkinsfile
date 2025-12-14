pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    environment {
        IMAGE_NAME = "jalelnasr/student-management"
        IMAGE_TAG  = "1.0.0"
    }

    stages {

        stage('Checkout GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/jalelnasr/ProjetStudentsManagement.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline Docker terminé avec succès'
        }
        failure {
            echo '❌ Erreur dans le pipeline'
        }
    }
}




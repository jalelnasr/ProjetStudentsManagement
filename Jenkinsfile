pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    stages {

        stage('Checkout') {
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
                sh 'docker build -t jalelnasr/student-management:1.0.0 .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f student-management || true
                docker run -d --name student-management -p 8080:8080 jalelnasr/student-management:1.0.0
                '''
            }
        }
    }
}

pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    stages {

        stage('Checkout') {
            steps {
                git url: 'https://github.com/jalelnasr/ProjetStudentsManagement.git', branch: 'master'
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
                docker rm -f student-management || true
                docker run -d --name student-management -p 9090:8089 jalelnasr/student-management:1.0.0
 
                '''
            }
        }
    }
}

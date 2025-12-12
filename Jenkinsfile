pipeline {
    agent any

    tools {
        maven 'M2_HOME'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/jalelnasr/ProjetStudentsManagement.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn -version'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'echo Running tests...'
            }
        }

        stage('Package') {
            steps {
                sh 'echo Packaging completed.'
            }
        }
    }
}


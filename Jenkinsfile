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

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                      echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                      docker push jalelnasr/student-management:1.0.0
                    '''
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                  docker rm -f student-management || true
                  docker run -d \
                    --name student-management \
                    -p 9090:8089 \
                    jalelnasr/student-management:1.0.0
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès'
        }
        failure {
            echo '❌ Pipeline échoué'
        }
    }
}



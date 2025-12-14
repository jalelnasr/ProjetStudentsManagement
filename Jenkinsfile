pipeline {
    agent any

    environment {
        IMAGE_NAME = "jalelnasr/student-management"
        IMAGE_TAG  = "1.0.0"
        KUBE_NAMESPACE = "devops"
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

        stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                      mvn sonar:sonar \
                      -Dsonar.projectKey=student-management \
                      -Dsonar.projectName=student-management \
                      -Dsonar.host.url=http://localhost:9000
                    '''
                }
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

        stage("Deploy to Kubernetes") {
            steps {
                sh '''
                  kubectl get ns ${KUBE_NAMESPACE} || kubectl create ns ${KUBE_NAMESPACE}

                  kubectl apply -f k8s/mysql-deployment.yaml -n ${KUBE_NAMESPACE}
                  kubectl apply -f k8s/mysql-service.yaml -n ${KUBE_NAMESPACE}

                  kubectl apply -f k8s/app-deployment.yaml -n ${KUBE_NAMESPACE}
                  kubectl apply -f k8s/app-service.yaml -n ${KUBE_NAMESPACE}

                  kubectl rollout status deployment/student-app -n ${KUBE_NAMESPACE} --timeout=120s
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD complet : Maven → Sonar → Docker → Kubernetes"
        }
        failure {
            echo "❌ Échec du pipeline"
        }
    }
}


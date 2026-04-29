pipeline {
    agent any

    tools {
        maven 'maven-3'
    }

    environment {
        APP_NAME = "cicd-demo"
        DOCKER_IMAGE = "cicd-demo-app:latest"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Compila la aplicacion Java omitiendo tests por agilidad
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Static Analysis (SonarQube)') {
            steps {
                // Esto cumple con el punto 3.1 del PDF
                withSonarQubeEnv('SonarQube') {
                    sh 'mvn sonar:sonar -Dsonar.projectKey=cicd-demo-app'
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }

        stage('Container Security Scan (Trivy)') {
            steps {
                // Ejecuta Trivy y detiene el pipeline si encuentra vulnerabilidades (--exit-code 1)
                sh "trivy image --exit-code 1 --severity HIGH,CRITICAL ${DOCKER_IMAGE}"
            }
        }

        stage('Deploy') {
            steps {
                // Elimina contenedor anterior si existe y despliega en el puerto 80 del host
                sh 'docker rm -f cicd-app || true'
                sh "docker run -d --name cicd-app -p 80:8080 ${DOCKER_IMAGE}"
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}


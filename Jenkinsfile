pipeline {
    agent any

    environment {
        IMAGE_NAME = "student-management"
        CONTAINER_NAME = "student-app"
        HOST_PORT = "8081"
        CONTAINER_PORT = "8089"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sahlihamza/DevOps_Project.git'
            }
        }

        stage('Build Maven') {
            steps {
                echo "📦 Build Maven en cours..."
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                echo "🐳 Création de l'image Docker..."
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Docker Run') {
            steps {
                echo "🚀 Lancement du conteneur Docker..."
                // Supprimer l'ancien conteneur s'il existe
                sh "docker rm -f ${CONTAINER_NAME} || true"
                // Lancer le conteneur avec mapping des ports
                sh "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo "✔️ Build Maven et déploiement Docker réussis !"
        }
        failure {
            echo "❌ Pipeline échoué !"
        }
    }
}

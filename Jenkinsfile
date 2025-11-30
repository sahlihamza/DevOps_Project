pipeline {
    agent any

    environment {
        IMAGE_NAME = "student-management"
        CONTAINER_NAME = "student-app"
        HOST_PORT = "8081"
        CONTAINER_PORT = "8089"
        SONARQUBE_SERVER = "MySonarQube" // Nom du serveur SonarQube défini dans Jenkins
    }

    stages {
        // 1️⃣ Récupérer le code depuis Git
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/sahlihamza/DevOps_Project.git'
            }
        }

        // 2️⃣ Maven clean & compile pour préparer le projet
        stage('Maven Clean & Compile') {
            steps {
                echo "🔧 Maven Clean et Compile..."
                sh 'mvn clean compile'
            }
        }

        // 3️⃣ Analyse SonarQube
        stage('SonarQube Analysis') {
            steps {
                echo "🔍 Analyse SonarQube en cours..."
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                       mvn sonar:sonar \
                         -Dsonar.projectKey=student-management \
                         -Dsonar.host.url=http://172.23.185.68:9000 \
                         -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
}

        // 4️⃣ Build Maven pour générer le JAR
        stage('Build Maven') {
            steps {
                echo "📦 Build Maven pour générer le JAR..."
                sh 'mvn package -DskipTests'
                sh 'ls -l target/'
            }
        }

        // 5️⃣ Création de l'image Docker (après SonarQube)
        stage('Docker Build') {
            steps {
                echo "🐳 Création de l'image Docker..."
                sh 'ls -l'
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        // 6️⃣ Lancement du conteneur Docker
        stage('Docker Run') {
            steps {
                echo "🚀 Lancement du conteneur Docker..."
                sh "docker rm -f ${CONTAINER_NAME} || true"
                sh "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} ${IMAGE_NAME}"
            }
        }
    }

    post {
        success {
            echo "✔️ Pipeline terminé avec succès : Build Maven, SonarQube et Docker"
        }
        failure {
            echo "❌ Pipeline échoué !"
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/sahlihamza/DevOps_Project.git'
                sh 'mvn clean package -DskipTests'

            }
        }
    }
    post {
        success {
            echo "✔️ Build réussii"
        }
        failure {
            echo "❌ Build échoué"
        }
    }
}

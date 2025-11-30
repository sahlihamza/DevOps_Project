pipeline {
    agent any
    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }
    stages {
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/sahlihamza/DevOps_Project.git'
                sh 'mvn clean package'
            }
        }
    }
    post {
        success {
            echo "✔️ Build réussi"
        }
        failure {
            echo "❌ Build échoué"
        }
    }
}

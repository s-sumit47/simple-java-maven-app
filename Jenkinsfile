pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -U clean package -DskipTests' 
            }
        }
    }
}

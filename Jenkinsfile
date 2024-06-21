pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -U clean package -DskipTests' 
            }
        }
    }
}

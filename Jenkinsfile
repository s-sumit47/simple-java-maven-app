pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                bat 'mvn -U -B -DskipTests clean package'
' 
            }
        }
    }
}

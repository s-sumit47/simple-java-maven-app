pipeline {
    agent any
    
    options {
        skipStagesAfterUnstable()
    }
    
    stages {
        stage('Build') {
            steps {
                bat 'mvn -B -DskipTests clean package'
            }
        }
        
        stage('Test') {
            steps {
                bat 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Deliver') {
            steps {
                bat '.\\jenkins\\scripts\\deliver.sh'
            }
        }
        
        stage('SonarQube Analysis') {
            environment {
                // Define environment variable to hold Maven home path
                MVN_HOME = tool name: 'Default Maven', type: 'maven'
            }
            steps {
                // Checkout SCM if needed
                checkout scm
                
                // Execute SonarQube analysis with Maven
                withSonarQubeEnv('sonar') {
                    sh "${MVN_HOME}/bin/mvn clean verify sonar:sonar"
                }
            }
        }
    }
}

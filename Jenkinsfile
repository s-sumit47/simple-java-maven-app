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
            steps {
                // Define Maven tool within the steps block
                def mvnHome = tool name: 'Default Maven', type: 'maven'
                
                // Checkout SCM (if needed)
                checkout scm
                
                // Execute SonarQube analysis with Maven
                withSonarQubeEnv('sonar') {
                    sh "${mvnHome}/bin/mvn clean verify sonar:sonar"
                }
            }
        }
    }
}

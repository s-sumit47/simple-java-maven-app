pipeline {
    agent any
    
    options {
        skipStagesAfterUnstable()
    }
    
    stages {
        stage('Build') {
            steps {
                // Execute Maven to clean and package the project
                bat 'mvn -B -DskipTests clean package'
            }
        }
        
        stage('Test') {
            steps {
                // Run Maven to execute tests
                bat 'mvn test'
            }
            post {
                always {
                    // Publish JUnit test results
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        
        stage('Deliver') {
            steps {
                // Execute delivery script
                bat '.\\jenkins\\scripts\\deliver.sh'
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                // Check out source code
                checkout scm
                
                // Define Maven tool
                def mvnHome = tool 'Default Maven'
                
                // Execute SonarQube analysis with Maven
                withSonarQubeEnv {
                    sh "${mvnHome}/bin/mvn clean verify sonar:sonar"
                }
            }
        }
    }
}

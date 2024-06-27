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
                MVN_HOME = tool name: 'Default Maven', type: 'maven'
            }
            steps {
                checkout scm
                
                withSonarQubeEnv('sonar') {
                    bat "${MVN_HOME}/bin/mvn clean verify sonar:sonar"
                }
            }
        }
        
        stage('Deploy to Tomcat') {
            steps {
                // Copy the WAR file to the Tomcat webapps directory using Windows command
                bat "copy target\\*.war C:\Program Files\Apache Software Foundation\Tomcat 9.0\webapps"
                
                // Restart Tomcat (optional, if needed)
                bat "net stop TomcatServiceName"
                bat "net start TomcatServiceName"
                
                // Deploy using the Deploy to container Plugin
                deploy(
                    adapters: [
                        tomcat8(credentialsId: 'admin', url: 'http://localhost:8090/')
                    ],
                    contextPath: '', // Optional: Specify context path if needed, e.g., '/myapp'
                    war: 'target/*.war'
                )
            }
        }
    }
}

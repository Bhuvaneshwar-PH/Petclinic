pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'mvn'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Bhuvaneshwar-PH/Petclinic.git'
            }
        }
        stage('install') {
            steps {
              sh "mvn clean compile"
            }
        }
         stage('test') {
            steps {
              sh "mvn test -fn jacoco:prepare-agent jacoco:report"
            }
        }
    

        stage('Sonarqube Analysis') {
            steps {
                
                    withSonarQubeEnv('sonar-server') {
                        sh """
                            $SCANNER_HOME/bin/sonar-scanner \
                            -Dsonar.projectName=petclinic \
                            -Dsonar.projectKey=petclinic \
                            -Dsonar.java.binaries=. \
                            -Dsonar.jacoco.reportPaths=target/site/jacoco/jacoco.xml
                        
                            
                        """
                    }
                
            }
        }
    } 
}

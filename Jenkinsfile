pipeline {
    agent any
    environment {
        DOCKER_CREDS = credentials('docker-credentials')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
  
        stage('build') {
            agent {
                 docker { image 'maven:3.6.3-openjdk-11-slim' }
            }
            steps {
                sh 'mvn clean install'
            }
        }    
        stage('SonarQube') {
            agent {
                docker { image 'maven:3.6.3-openjdk-11-slim' }
            }
            steps {
                def scannerHome = tool 'scanner-default'
                withSonarQubeEnv('sonar-server') {
                    sh "${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=labgradle01 \
                        -Dsonar.projectName=labgradle01 \
                        -Dsonar.sources=src/main/kotlin \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.tests=src/test/kotlin"
                }
            }
         }

    }
}

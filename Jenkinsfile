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
                git branch: 'main', credentialsId: 'git_hub', url: 'https://github.com/aldo2510/galaxy-jenkins-lab-maven.git'
            }
         }

         stage('Build docker image') {
            agent any
            steps {
                sh 'docker build -t galaxy-examen .'
            }
        }
        stage('Push docker image') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable:'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                    sh 'docker push galaxy-examen'
                }
            }
        }
        stage('Run Container') {
            agent any
            steps {
                sh 'docker run -d -p 8095:8080 galaxy-examen'
            }
        }
    }
}

pipeline {
    agent any

    environment {
        SONAR_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/kunals30/spring-boot-thymeleaf-example.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    $SONAR_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=springboot-app \
                    -Dsonar.projectName="Spring Boot App" \
                    -Dsonar.sources=. \
                    -Dsonar.java.binaries=target
                    '''
                }
            }
        }

    }
}

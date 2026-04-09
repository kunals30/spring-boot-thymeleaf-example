pipeline {
    agent any

    environment {
        SONAR_HOME = tool 'SonarScanner'
        DOCKER_IMAGE = "13.232.140.129:8083/docker-repo/springboot-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/kunals30/spring-boot-thymeleaf-example.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
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
                    -Dsonar.sources=. \
                    -Dsonar.java.binaries=target
                    '''
                }
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-docker',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh 'echo $PASSWORD | docker login 13.232.140.129:8083 -u $USERNAME --password-stdin'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

    }
}

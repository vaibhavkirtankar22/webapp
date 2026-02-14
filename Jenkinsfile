pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/Aseemakram19/java-maven-app.git'
            }
        }

        stage('Build WAR') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t success222/java-app:latest .'
            }
        }

        stage('Push Image') {
            steps {
                withCredentials([usernamePassword(
                credentialsId: 'dockerhub',
                usernameVariable: 'USER',
                passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push success222/java-app:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f java-app || true
                docker run -d --name java-app -p 80:8080 success222/java-app:latest
                '''
            }
        }
    }
}

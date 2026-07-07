pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test || true'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                pkill -f devops-demo || true
                nohup /usr/lib/jvm/java-11-openjdk-amd64/bin/java \
                -jar target/devops-demo-0.3.0.jar \
                > app.log 2>&1 &
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                sleep 10
                curl http://localhost:8080/sayhi
                '''
            }
        }
    }
}

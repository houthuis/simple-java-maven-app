pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver for development') {
            when {
                branch 'dev'
            }
            steps {
                sh './jenkins/scripts/deliver_dev.sh'
            }
        }
        stage('Deliver for production') {
            when {
                branch 'prod'
            }
            steps {
                sh './jenkins/scripts/deliver_prod.sh'
            }
        }
    }
}
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/ozayash22/aws_Practical.git', credentialsId: 'github-credentials'
            }
        }

        stage('Build') {
            steps {
                sh '''
                javac HelloJenkins.java
                '''
            }
        }

        stage('Run') {
            steps {
                sh '''
                java HelloJenkins
                '''
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to /var/www/html'
                sh '''
                sudo rm -rf /var/www/html/*
                sudo cp HelloJenkins.java /var/www/html/
                '''
            }
        }
    }

    post {
        success {
            echo 'Build and Deploy successful!'
        }
        failure {
            echo 'Build or Deploy failed.'
        }
    }
}

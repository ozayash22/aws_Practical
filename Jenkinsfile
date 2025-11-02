pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/<your-repo>.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                # Stop any existing app
                pkill -f myapp.jar || true

                # Copy the new jar to /opt/app
                sudo mkdir -p /opt/app
                sudo cp target/myapp.jar /opt/app/

                # Run the jar in background
                nohup java -jar /opt/app/myapp.jar > /opt/app/app.log 2>&1 &
                '''
            }
        }
    }
}

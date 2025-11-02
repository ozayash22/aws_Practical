pipeline {
  agent any

  environment {
    WEBROOT = '/var/www/html'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build (optional)') {
      steps {
        // If you have build steps (npm build etc.), add them here. For static site, skip
        sh '''
        if [ -f package.json ]; then
          npm install
          npm run build || true
        fi
        '''
      }
    }

    stage('Deploy') {
      steps {
        // Use sudo cp -> requires jenkins passwordless sudo configured earlier
        sh '''
        echo "Deploying to ${WEBROOT}"
        # remove old contents (be careful)
        sudo rm -rf ${WEBROOT}/*
        # copy repo files to webroot (exclude .git)
        sudo cp -r `pwd`/* ${WEBROOT}/
        # fix ownership (optional)
        sudo chown -R nginx:nginx ${WEBROOT} || sudo chown -R ec2-user:ec2-user ${WEBROOT} || true
        # restart nginx
        sudo systemctl restart nginx
        '''
      }
    }
  }

  post {
    success {
      echo "Deployment successful"
    }
    failure {
      echo "Deployment failed"
    }
  }
}

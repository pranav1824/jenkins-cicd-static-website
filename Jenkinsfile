
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validation') {
            steps {
                sh '''
                    set -eu
                    test -s index.html
                    grep -qi '<html' index.html
                    echo "HTML validation passed"
                '''
            }
        }

        stage('Deployment') {
            steps {
                sh '''
                    set -eu
                    test -d /var/www/cicd-site
                    rsync -av --delete \
                      --exclude='.git/' \
                      --exclude='Jenkinsfile' \
                      ./ /var/www/cicd-site/
                    test -s /var/www/cicd-site/index.html
                    echo "Deployment successful"
                '''
            }
        }
    }
}

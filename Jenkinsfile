// On every push, GitHub calls the Jenkins webhook and this pipeline runs on the EC2 master.
pipeline {
    agent any
    triggers { githubPush() }

    stages {
        stage('Build') {
            steps {
                // stamp the page with the build number, commit and time
                sh '''
                    sed -i "s/__BUILD__/${BUILD_NUMBER}/" index.html
                    sed -i "s/__COMMIT__/$(git rev-parse --short HEAD)/" index.html
                    sed -i "s/__TIME__/$(date '+%Y-%m-%d %H:%M:%S')/" index.html
                '''
            }
        }
        stage('Publish') {
            steps {
                sh 'cp index.html /var/www/html/index.html'   // nginx serves this folder
            }
        }
        stage('Verify') {
            steps {
                sh 'curl -s http://localhost | grep "Jenkins build"'
            }
        }
    }
}

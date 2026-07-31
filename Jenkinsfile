pipeline {
    agent any

    environment {
        DOCKER_REGISTRY_CREDS = 'docker-jenkins-token-1'
        DOCKER_BFLASK_IMAGE = 'ymnddisanayaka/myjava1:latest'
    }

    stages {
        stage('Build') {
            steps {
                bat 'docker build -t my-flask-app .'
                bat 'docker tag my-flask-app %DOCKER_BFLASK_IMAGE%'
            }
        }

        stage('Test') {
            steps {
                bat 'docker run --rm my-flask-app python -m pytest app/tests/'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKER_REGISTRY_CREDS}",
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    bat 'echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin'
                    bat 'docker push %DOCKER_BFLASK_IMAGE%'
                }
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}

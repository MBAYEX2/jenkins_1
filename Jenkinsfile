pipeline {
    agent any

    environment {
        DOCKER_USER = 'arafat2'
        BACKEND_IMAGE = "${DOCKER_USER}/appprof-frontend"
        FRONTEND_IMAGE = "${DOCKER_USER}/appprof-backend"
        MIGRATE_IMAGE = "${DOCKER_USER}/appprof-migrate"
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/MBAYEX2/jenkins_1.git'
            }
        }

        stage('Build des images') {
    steps {
        sh 'docker build -t $BACKEND_IMAGE:latest ./Backend/odc'
        sh 'docker build -t $FRONTEND_IMAGE:latest ./Frontend'
        sh 'docker build -t $MIGRATE_IMAGE:latest ./Backend/odc'
    }
}

        

       stage('Push des images sur Docker Hub') {
    steps {
        withDockerRegistry([credentialsId: 'accjenkins', url: '']) {
            sh 'docker push $BACKEND_IMAGE:latest'
            sh 'docker push $FRONTEND_IMAGE:latest'
            sh 'docker push $MIGRATE_IMAGE:latest'
        }
    }
}


        stage('Déploiement Local avec Docker Compose') {
            steps {
                sh '''
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            echo "✅ CI/CD terminé avec succès"
        }
        failure {
            echo "❌ Échec du pipeline"
        }
    }
}

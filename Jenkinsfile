pipeline {
    agent any
    environment {
        COMPOSE_TEMPLATE_FILE = 'docker-compose.yml'
        ENV_DIR = 'env/dev.env'
        PROJECT_NAME = 'dev_pg'
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to  Environment') {
            steps {
                script {
                    echo "Deploy from ${env.BRANCH} branch"
                    sh "docker compose -f ${COMPOSE_TEMPLATE_FILE} --project-name ${PROJECT_NAME} --env-file ${ENV_DIR} up -d --remove-orphans"
                }
            }
        }
        stage('Cleanup Docker') {
            steps {
                echo "Pruning unused Docker Images"
                sh "docker image prune -f"
            }
        }
    }
}
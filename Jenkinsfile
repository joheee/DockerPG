pipeline {
    agent {
        docker {
            image "docker:latest"
            args "-v /var/run/docker.sock:/var/run/docker.sock"
        }
    }
    environment {
        COMPOSE_TEMPLATE_FILE = 'docker-compose.yml'
        ENV_DIR = 'env/dev.env'
        PROJECT_NAME = 'dev_pg'
    }
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to  Environment') {
            steps {
                script {
                    echo "Deploy from ${env.BRANCH_NAME} branch"
                    sh "docker compose -f ${COMPOSE_TEMPLATE_FILE} --project-name ${PROJECT_NAME} --env-file ${ENV_DIR} down"
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
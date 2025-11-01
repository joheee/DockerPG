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
    post {
        failure {
            emailext (
                to: 'johevinblesstowi07@gmail.com', 
                subject: "FAILED: Pipeline '${currentBuild.fullDisplayName}'",
                body: """<p>CHECK FAILED PIPELINE: <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
                         <p>The build for branch <b>${env.BRANCH_NAME}</b> has failed.</p>
                         <p>Please check the console output for more details.</p>"""
            )
        }

        success {
            emailext (
                to: 'johevinblesstowi07@gmail.com',
                subject: "SUCCESS: Pipeline '${currentBuild.fullDisplayName}'",
                body: """<p>SUCCESSFUL DEPLOYMENT: <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
                         <p>The build for branch <b>${env.BRANCH_NAME}</b> was deployed successfully.</p>"""
            )
        }
    }
}
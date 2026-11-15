pipeline {
    agent any

    options {
        disableConcurrentBuilds()
    }

    stages {

        stage('Authentification Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKERHUB_USER',
                        passwordVariable: 'DOCKERHUB_TOKEN'
                    )
                ]) {
                    sh '''
                        set +x
                        echo "$DOCKERHUB_TOKEN" | docker login \
                            -u "$DOCKERHUB_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Verification du Polling SCM') {
            steps {
                echo 'POLLING SCM : nouvelle modification detectee depuis GitHub !'
                echo 'CLINIC-CD - Version de verification 1.1'
            }
        }

        stage('Recuperation des images') {
            steps {
                sh '''
                    docker compose \
                      -p deployment-cd \
                      -f /opt/dentardv/deployment-cd/docker-compose.yml \
                      --env-file /opt/dentardv/deployment-cd/.env \
                      pull backend frontend
                '''
            }
        }

        stage('Deploiement automatique') {
            steps {
                sh '''
                    docker compose \
                      -p deployment-cd \
                      -f /opt/dentardv/deployment-cd/docker-compose.yml \
                      --env-file /opt/dentardv/deployment-cd/.env \
                      up -d --no-build --remove-orphans
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }

        success {
            echo 'SUCCESS : CLINIC deploye avec les nouvelles images Docker Hub.'
        }

        failure {
            echo 'ECHEC : verifier les journaux du pipeline CD.'
        }
    }
}

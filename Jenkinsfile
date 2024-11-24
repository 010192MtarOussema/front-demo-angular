pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out code...'
                checkout scm
                echo "Current branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Install Application') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'npm run test --watch=false --browsers=ChromeHeadless'
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building application for production...'
                bat 'npm run build '
            }
        }

        stage('Deploy Decision') {
            when {
                not {
                    branch 'main'
                }
            }
            steps {
                script {
                    def deploy = input(
                        id: 'Deploy',
                        message: "La branche actuelle est '${env.BRANCH_NAME}', voulez-vous déployer dans cet environnement ?",
                        parameters: [
                            booleanParam(
                                defaultValue: false,
                                description: 'Cochez pour confirmer le déploiement.',
                                name: 'Deploy'
                            )
                        ]
                    )

                    if (!deploy) {
                        error("Déploiement annulé par l'utilisateur pour la branche '${env.BRANCH_NAME}'.")
                    } else {
                        echo "Déploiement autorisé pour la branche '${env.BRANCH_NAME}'."
                    }
                }
            }
        }

        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    expression { return inputWasApproved() }
                }
            }
            steps {
                echo 'Deploying application...'
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'VM-Centos', // Nom défini dans Publish Over SSH
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'dist/**/*', // Répertoire Angular généré après le build
                                    remoteDirectory: '/var/www/html', // Répertoire sur le serveur distant
                                    execCommand: '''
                                        echo "Clearing old application files..."
                                        rm -rf /var/www/html/*
                                        echo "Deploying new application..."
                                        mv dist/* /var/www/html/
                                        echo "Deployment completed successfully."
                                    '''
                                )
                            ]
                        )
                    ]
                )
            }
        }
    }

    post {
        always {
            echo "========always========"
        }
        success {
            echo "========pipeline executed successfully========"
        }
        failure {
            echo "========pipeline execution failed========"
        }
        aborted {
            echo "========pipeline execution was aborted========"
        }
    }
}

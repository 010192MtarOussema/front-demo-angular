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
                bat 'npm run build'
            }
        }

        stage('Deploy to NGINX') {
            steps {
                echo "Deploying application for branch '${env.BRANCH_NAME}'..."
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'VM-Centos', // Nom défini dans Publish Over SSH
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'dist/**/*', // Répertoire Angular généré après le build
                                    remoteDirectory: '/usr/share/nginx/html', // Répertoire par défaut de NGINX pour les fichiers web
                                    execCommand: '''
                                        echo "Clearing old application files..."
                                        sudo rm -rf /usr/share/nginx/html/*
                                        
                                        echo "Deploying new application files..."
                                        sudo cp -r * /usr/share/nginx/html/
                                        
                                        echo "Restarting NGINX to apply changes..."
                                        sudo systemctl restart nginx
                                        
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
            echo "Pipeline terminé, quelle que soit l'issue."
        }
        success {
            echo "Pipeline exécuté avec succès."
        }
        failure {
            echo "Pipeline échoué."
        }
    }
}

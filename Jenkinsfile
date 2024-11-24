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

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install'
            }
        }

        stage('Build Angular Application') {
            steps {
                echo 'Building Angular application for production...'
                bat 'npm run build'
            }
        }

        stage('Deploy to NGINX') {
            steps {
                echo "Deploying Angular application to NGINX..."
                sshPublisher(
                    publishers: [
                        sshPublisherDesc(
                            configName: 'VM-Centos', // Nom défini dans Publish Over SSH
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/dist/front-demo-angular/browser/**/*', // Fichiers générés par Angular
                                    remoteDirectory: '/usr/share/nginx/html', // Répertoire cible sur le serveur CentOS
                                    execCommand: '''
                                        echo "Checking current files on server..."
                                        ls -la /usr/share/nginx/html

                                        echo "Clearing old application files..."
                                        sudo rm -rf /usr/share/nginx/html/*

                                        echo "Deploying new application files..."
                                        ls -la # Vérification des fichiers avant copie
                                        sudo cp -r * /usr/share/nginx/html/
                                        ls -la /usr/share/nginx/html # Vérification des fichiers après copie

                                        echo "Restarting NGINX service..."
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

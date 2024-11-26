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

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'npm run test -- --watch=false --code-coverage'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo 'Running SonarQube analysis...'
                withSonarQubeEnv('SonarQube') { // Nom du serveur configuré dans Jenkins
                    bat '''
                        sonar-scanner.bat ^
                        -Dsonar.projectKey=front-demo-angular ^
                        -Dsonar.sources=src ^
                        -Dsonar.exclusions=**/*.spec.ts,**/node_modules/** ^
                        -Dsonar.tests=src ^
                        -Dsonar.test.inclusions=**/*.spec.ts ^
                        -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    '''
                }
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
                            configName: 'VM-Centos',
                            transfers: [
                                sshTransfer(
                                    sourceFiles: '**/dist/front-demo-angular/browser/**/*',
                                    remoteDirectory: '/usr/share/nginx/html',
                                    execCommand: '''
                                        echo "Clearing old application files..."
                                        sudo rm -rf /usr/share/nginx/html/*

                                        echo "Deploying new application files..."
                                        sudo cp -r * /usr/share/nginx/html/

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

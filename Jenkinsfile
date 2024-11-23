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
                echo 'Running tests...'
                bat 'npm run install '
            }
        }
        stage('Run Tests') {
            steps {
                echo 'Running tests...'
                bat 'npm run test '
            }
        }

        stage('Build Application') {
            steps {
                echo 'Building application for production...'
             
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo 'Deploying application to production...'

             
            }
        }
    }
    stages{
        stage("A"){
            steps{
                echo "========executing A========"
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
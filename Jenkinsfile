pipeline {
    agent any

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/thrtt/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Verify .NET') {
            steps {
                bat 'dotnet --version'
            }
        }

        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build project') {
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release --no-restore'
            }
        }

        stage('Run tests') {
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=test_results.trx" --no-build'
            }
        }
    }

    post {
        always {
            // Архивиране на тестовите резултати
            junit 'SeleniumIde/**/TestResults/*.trx'
        }
    }
}

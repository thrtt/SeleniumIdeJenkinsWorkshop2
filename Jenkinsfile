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

        stage('Run tests - SeleniumIde') {
            steps {
                bat '''
                mkdir SeleniumIDE\\TestResults || echo TestResults exists
                dotnet test SeleniumIde.sln --configuration Release --logger "trx;LogFileName=SeleniumIDE\\TestResults\\SeleniumIde_test_results.trx" --no-build
                '''
            }
        }

        // Ако имаш други test проекти, можеш да добавиш тук допълнителни stages
        // stage('Run tests - TestProject1') { ... }

    }

    post {
        always {
            echo 'Archiving test results...'
            junit 'SeleniumIDE/TestResults/*.trx'
        }
    }
}

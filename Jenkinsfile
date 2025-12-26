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
                mkdir TestResults || echo TestResults exists
                dotnet test SeleniumIde.sln --configuration Release --logger "trx;LogFileName=TestResults\\SeleniumIde_test_results.trx" --no-build
                '''
            }
        }

        stage('Run tests - TestProject1') {
            steps {
                bat '''
                mkdir TestProject1\\TestResults || echo TestResults exists
                dotnet test TestProject1\\TestProject1.csproj --configuration Release --logger "trx;LogFileName=TestProject1\\TestResults\\TestProject1_test_results.trx" --no-build
                '''
            }
        }

        stage('Run tests - TestProject2') {
            steps {
                bat '''
                mkdir TestProject2\\TestResults || echo TestResults exists
                dotnet test TestProject2\\TestProject2.csproj --configuration Release --logger "trx;LogFileName=TestProject2\\TestResults\\TestProject2_test_results.trx" --no-build
                '''
            }
        }
    }

    post {
        always {
            echo 'Archiving test results...'
            junit 'SeleniumIde/TestResults/*.trx'
            junit 'TestProject1/TestResults/*.trx'
            junit 'TestProject2/TestResults/*.trx'
        }
    }
}

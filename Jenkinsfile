pipeline {
    agent any

    stages {

        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/thrtt/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Setup .NET') {
            steps {
                bat '''
                echo Setting up .NET 8.0 SDK
                choco install dotnet-8.0-sdk -y
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                bat '''
                echo Restoring .NET dependencies
                dotnet restore
                '''
            }
        }

        stage('Build project') {
            steps {
                bat '''
                echo Building the project
                dotnet build --configuration Release
                '''
            }
        }

        stage('Run tests') {
            steps {
                bat '''
                echo Running tests
                dotnet test SeleniumIde.sln --logger "trx;LogFileName=test_results.trx"
                '''
            }
        }
    }

    post{
        aways{
            archiveArtifacts artifacts: '*/TestResults/*.trx', allowEmptyArchive: true
            junit '*/TestResults/*.trx' 
        }
    }
}

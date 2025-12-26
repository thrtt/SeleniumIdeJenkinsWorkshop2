pipeline {
    agent any

    stages {

        stage('Checkout code') {
            steps {
                git branch: 'main', 
                url: 'https://github.com/thrtt/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Setup .NET') {
            steps {
                bat '''
                echo Checking up .NET 8.0 SDK instalation...
                donet --info
                '''
            }
        }

        stage('Restore dependencies') {
            steps {
                bat '''
                echo Restoring .NET dependencies
                dotnet restore 'SeleniumIde.sln
                '''
            }
        }

        stage('Build project') {
            steps {
                bat '''
                echo Building the project
                dotnet build  --configuration Release
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
        always{
            archiveArtifacts artifacts: '*/TestResults/*.trx', allowEmptyArchive: true
            junit '*/TestResults/*.trx' 
        }
    }
}

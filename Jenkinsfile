pipeline {
    agent any

    tools {
        // Увери се, че името 'dotnet-8' съвпада точно с това в Global Tool Configuration
        dotnetsdk 'dotnet-8'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/thrtt/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Restore & Build') {
            steps {
                bat 'dotnet restore'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Run tests') {
            steps {
                // Използваме тестов логър и гарантираме създаването на директорията
                bat 'dotnet test --configuration Release --no-build --logger "trx;LogFileName=test_results.trx" --results-directory TestResults'
            }
        }
    }

    post {
        always {
            // Тези стъпки трябва да са тук, за да имат достъп до файловете
            script {
                archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
                // Ако нямаш MSTest плъгин, закоментирай долния ред, за да не гърми
                // mstest testResultsFile: '**/TestResults/*.trx'
            }
        }
    }
}
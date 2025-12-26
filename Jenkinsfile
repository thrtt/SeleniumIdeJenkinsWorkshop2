pipeline {
    agent any

    tools {
        // Това име трябва да съвпада точно с името, което даде в Стъпка 1
        dotnetsdk 'dotnet-8'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/thrtt/SeleniumIdeJenkinsWorkshop2.git'
            }
        }

        stage('Restore dependencies') {
            steps {
                bat 'dotnet restore'
            }
        }

        stage('Build project') {
            steps {
                bat 'dotnet build --configuration Release --no-restore'
            }
        }

        stage('Run tests') {
            steps {
                // Използваме конкретна директория за резултатите, за да сме сигурни къде отиват
                bat 'dotnet test --configuration Release --no-build --logger "trx;LogFileName=results.trx" --results-directory TestResults'
            }
        }
    }

    post {
        always {
            // Архивира .trx файла, за да можеш да го свалиш по-късно
            archiveArtifacts artifacts: 'TestResults/*.trx', allowEmptyArchive: true
            
            // MSTest плъгинът визуализира тестовете в Jenkins (препоръчително)
            mstest testResultsFile: 'TestResults/*.trx', keepLongStdio: true
        }
    }
}
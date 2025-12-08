pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/EmiliyaKamburova/SeleniumIDE_Workshop_1.git'
            }
        }

        stage('Setup .Net Environment') {
            steps {
                bat '''
                echo Using system installed .NET SDK
                dotnet --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                echo Restoring dependencies
                dotnet restore
                '''
            }
        }

        stage('Build project') {
            steps {
                bat '''
                echo Building project
                dotnet build --no-restore
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

    post {
        always {
            archiveArtifacts artifacts: '*/TestResults/*.trx', allowEmptyArchive: true

            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx',
                failOnError: false
            ])
        }
    }
}

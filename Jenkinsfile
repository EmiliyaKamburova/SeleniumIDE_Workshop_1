pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
               git brancjh: 'main', url: 'https://github.com/EmiliyaKamburova/SeleniumIDE_Workshop_1.git'
            }

        stage('Setup .Net Environment') {
            steps {
                bat '''
                echo Setting up .Net 8.0 SDK
                choko install dotnet-8.0-sdk -y
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
            archiveArtifacts '*/TestResults/*.trx', allowEmptyArchive: true
            step([$class: 'MSTestPublisher', 
            testResultsFile: '**/TestResults/*.trx'
            ])            
        }
    }     
}
pipeline {
    agent any

    stages {
        stage("Checkout code") {
            steps {
                // Checkout the repository
                git branch: 'main', url: "https://github.com/CooLVasileff/JenkinsSeleniumWebDriverTests_30_07.git"
            }
        }
        stage("Set up .Net core") {
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -L -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
                echo Installing SDK
                dotnet-sdk-6.0.132-win-x64.exe /quiet /norestart
                '''
            }
        }
        stage("Restoring nuget packages") {
            steps {
                // Restore dependencies
                bat 'dotnet restore SeleniumBasicExercise.sln'
            }
        }
        stage("Build") {
            steps {
                // Build solution
                bat 'dotnet build SeleniumBasicExercise.sln --configuration Release'
            }
        }
        stage("Run tests TestProject1") {
            steps {
                // Run tests
                bat 'dotnet test TestProject1/TestProject1.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
        stage("Run tests TestProject2") {
            steps {
                // Run tests
                bat 'dotnet test TestProject1/TestProject2.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
        stage("Run tests TestProject3") {
            steps {
                // Run tests
                bat 'dotnet test TestProject1/TestProject3.csproj --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            mstest testResultsFile: '**/TestResults/*.trx'
        }
    }
}

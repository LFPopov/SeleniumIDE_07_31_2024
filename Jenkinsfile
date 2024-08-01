pipeline {
    agent any

    stages {

        stage("Checkout code") {
            steps {
                git branch: 'main', url: 'https://github.com/LFPopov/SeleniumIDE_07_31_2024'
            }            
        }
        stage("setup .NET core") {
            steps{
                bat '''
                echo downloading .NET 6.0.132
                curl -l -o dotnet-sdk-6.0.132-win-x64.exe https://download.visualstudio.microsoft.com/download/pr/0c82e7e6-fdde-49f2-a69f-bd986aeafe1b/9ea7411a22e661fff0e61e56a466e484/dotnet-sdk-6.0.132-win-x64.exe
                echo Installing dotnet-sdk-6.0.132-win-x64.exe
                dotnet-sdk-6.0.132-win-x64.exe  
                '''
                }
        }
        stage("Restoring nuget packages") {
            steps{
                //Restore dependencies using the solution file
                bat 'dotnet restore SeleniumIde.sln'
                }
        } 
         stage("Build") {
            steps{
                bat 'dotnet build SeleniumIde.sln --configuration release'
                }
        }
        stage("Run tests") {
            steps{
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
                }
        }                                                
        }
        
    post {
        always {
            archiveArtifacts artifacts: '**/TestResults.trx/*', allowEmptyArchive: true
            steps {
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults.trx/*'
            }
        }
    }

}
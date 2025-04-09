pipeline{
  agent any
  stages{
    stage('Checkout code'){
      steps{
        git branch: 'main', url: 'https://github.com/CrimsonKingCodes/JenkinsSeleniumIDEApril2025'
      }
    }

    stage('Setup DotNet core'){
      steps{
        bat '''
        echo Downloading .Net 6 Sdk
        curl -l -o dotnet-sdk-6.0.428-win-x86.exe https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.428/dotnet-sdk-6.0.428-win-x86.exe
        echo installing dotnet-sdk-6.0.428-win-x86.exe
        dotnet-sdk-6.0.428-win-x86.exe /quiet /norestart
        '''
      }
    }

    stage('SRestoring NuGet packages'){
      steps{
        bat 'dotnet restore SeleniumIde.sln'
      }
    }

    stage('Build'){
      steps{
        bat 'dotnet build SeleniumIde.sln --configuration Release'
      }
    }

    stage('Run Tests'){
      steps{
        bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
      }
    }
  }

  post{
    always{
      archiveArtifacts arifacts: '**/TestResults/*.trx', allowEmptyArchive: true
      step([
        $class: 'MSTestPublisher',
        TestResultsFile: '**/TestResults/*.trx'
      ])
    }
  }
}
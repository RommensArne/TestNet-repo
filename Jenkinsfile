pipeline {
    agent any
    tools {
        dotnetsdk 'dotnet-sdk-8.0'
    }
    environment {
        PATH = "$PATH:/root/.dotnet/tools"
    }
    stages {
        stage('Setup Environment') {
            steps {
                echo 'Installing necessary packages...'
                sh 'apt-get update && apt-get install -y libicu-dev'
            }
        }
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'git@github.com:RommensArne/TestNet-repo.git'
            }
        }
        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }
        stage('Install dotnet ef Tool') {
            steps {
                echo 'Installing dotnet-ef tool...'
                sh 'dotnet tool install --global dotnet-ef'
            }
        }
        stage('Apply Migrations') {
            steps {
                echo 'Applying database migrations...'
                sh 'dotnet ef database update --startup-project Rise.Server --project Rise.Persistence'
            }
        }
        stage('Publish Rise.server') {
            steps {
                echo 'Publishing the application...'
                sh 'dotnet publish Rise.Server/Rise.Server.csproj -c Release -o ./publish/server'
                sh 'dotnet publish Rise.Client/Rise.Client.csproj -c Release -o ./publish/client'
                } 
            
            }
    }
    post {
        success {
            echo 'Pipeline completed successfully! Database created successfully!'
        }
        failure {
            echo 'Pipeline execution failed. Database creation failed.'
        }
    }
}
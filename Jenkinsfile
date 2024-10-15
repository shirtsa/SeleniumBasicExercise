pipeline {
    agent any
    triggers {
        // Trigger the pipeline on push or pull request to the main branch
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repo
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                // Restore dependencies
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                // Build the solution without restoring dependencies again
                bat 'dotnet build --no-restore'
            }
        }

        stage('Run Tests') {
            parallel {
                stage('Run UI Tests on Project 1') {
                    steps {
                        // Run tests on TestProject1
                        bat 'dotnet test TestProject1/TestProject1.csproj --no-build --verbosity normal'
                    }
                }
                stage('Run UI Tests on Project 2') {
                    steps {
                        // Run tests on TestProject2
                        bat 'dotnet test TestProject2/TestProject2.csproj --no-build --verbosity normal'
                    }
                }
                stage('Run UI Tests on Project 3') {
                    steps {
                        // Run tests on TestProject3
                        bat 'dotnet test TestProject3/TestProject3.csproj --no-build --verbosity normal'
                    }
                }
            }
        }
    }

    post {
        always {
            // Archive results, logs, or cleanup actions
            junit '**/TestResults/*.xml'
            cleanWs()
        }
    }
}

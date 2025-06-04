pipeline {
    agent any

    // tools {
        // (Optional) if you have a ".NET SDK" tool named "dotnet6" in Jenkins → Global Tool Configuration
        // dotnet 'dotnet6'
    // }

    environment {
        // Opt out of telemetry to speed up dotnet commands
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_SKIP_FIRST_TIME_EXPERIENCE = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                dir('project-zero/nunit-1') {
                    sh 'dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                dir('project-zero/nunit-1') {
                    sh 'dotnet build --configuration Release --no-restore'
                }
            }
        }

        stage('Test') {
            steps {
                dir('project-zero/nunit-1') {
                    sh 'dotnet test --configuration Release --no-build --logger:junit'
                }
            }
        }

        stage('Publish') {
            steps {
                dir('project-zero/nunit-1') {
                    sh 'dotnet publish --configuration Release --output out'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Archiving published artifacts for deployment...'
                archiveArtifacts artifacts: 'project-zero/nunit-1/out/**/*', fingerprint: true
            }
        }
    }

    post {
        always {
            // Correct usage: pass the XML‐glob directly, or use testResults:
            junit 'project-zero/nunit-1/TestResults/*.xml'
        }
    }
}

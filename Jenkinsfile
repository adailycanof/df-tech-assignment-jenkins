pipeline {
    agent any

    environment {
        // Opt out of telemetry to speed up dotnet commands
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_SKIP_FIRST_TIME_EXPERIENCE = '1'
        DOTNET_ROOT = '/tmp/dotnet'
        PATH = "${env.DOTNET_ROOT}:${env.PATH}"
    }

    stages {
        stage('Install .NET SDK') {
            steps {
                sh '''
                    if [ ! -d "/tmp/dotnet" ]; then
                        echo "Installing .NET 6.0 SDK..."
                        curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --version 6.0.425 --install-dir /tmp/dotnet
                    else
                        echo ".NET SDK already installed"
                    fi
                    /tmp/dotnet/dotnet --version
                '''
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                dir('project-zero/nunit-1') {
                    sh '/tmp/dotnet/dotnet restore'
                }
            }
        }

        stage('Build') {
            steps {
                dir('project-zero/nunit-1') {
                    sh '/tmp/dotnet/dotnet build --configuration Release --no-restore'
                }
            }
        }

        stage('Test') {
            steps {
                dir('project-zero/nunit-1') {
                    sh '/tmp/dotnet/dotnet test --configuration Release --no-build --logger:junit'
                }
            }
        }

        stage('Publish') {
            steps {
                dir('project-zero/nunit-1') {
                    sh '/tmp/dotnet/dotnet publish --configuration Release --output out'
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
            script {
                // Only try to publish test results if they exist
                if (fileExists('project-zero/nunit-1/TestResults/*.xml')) {
                    junit 'project-zero/nunit-1/TestResults/*.xml'
                } else {
                    echo 'No test results found'
                }
            }
        }
    }
}

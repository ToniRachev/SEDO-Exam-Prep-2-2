pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install .NET 6 SDK (if needed)') {
            steps {
                sh '''
                    if ! dotnet --list-sdks | grep -q 6.0; then
                        wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
                        chmod +x dotnet-install.sh
                        ./dotnet-install.sh --channel 6.0 --install-dir $DOTNET_ROOT
                    fi
                '''
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --configuration Release --verbosity normal'
            }
        }
    }
}

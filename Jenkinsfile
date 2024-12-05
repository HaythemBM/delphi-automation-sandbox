pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your/repo.git'
            }
        }

        stage('Build Source') {
            steps {
                script {
                    echo 'Building Delphi Source Project...'
                    bat """
                    "%WORKSPACE%\Utils\Build_10.4.bat" "%WORKSPACE%\Projects\Hello World\Source" HelloWorld.dproj Debug Win32 ".\Win32\Debug"
                    """
                }
            }
        }
        
        stage('Build Test') {
            steps {
                script {
                    echo 'Building Delphi Test Project...'
                    bat """
                    "%WORKSPACE%\Utils\Build_10.4.bat" "%WORKSPACE%\Projects\Hello World\Tests\DUnit" HelloWorldTests.dproj Debug Win32 ".\Win32\Debug"
                    """
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running Tests...'
                    bat """
                    cd %WORKSPACE%\Projects\Hello World\Tests\DUnit\Win32\Debug
                    HelloWorldTests.exe
                    """
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.exe', allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo 'Build and Test completed successfully.'
        }
        failure {
            echo 'Build or Test failed.'
        }
    }
}

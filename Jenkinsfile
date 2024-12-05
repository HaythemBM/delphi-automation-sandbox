pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/HaythemBM/delphi-automation-sandbox.git'
            }
        }

        stage('Build Source') {
            steps {
                script {
                    echo 'Building Delphi Source Project...'
                    /* groovylint-disable-next-line LineLength */
                    bat bat '"%WORKSPACE%\\Utils\\Build_10.4.bat" "%WORKSPACE%\\Projects\\Hello World\\Source" HelloWorld.dproj Debug Win32'
                }
            }
        }

        stage('Build Test') {
            steps {
                script {
                    echo 'Building Delphi Test Project...'
                    /* groovylint-disable-next-line LineLength */
                    bat '"%WORKSPACE%\\Utils\\Build_10.4.bat" "%WORKSPACE%\\Projects\\Hello World\\Tests\\DUnit" HelloWorldTests.dproj Debug Win32'

                }
            }
        }

        stage('Test') {
            steps {
                script {
                    echo 'Running Tests...'
                    bat '''
                    cd %WORKSPACE%\\Projects\\Hello World\\Tests\\DUnit\\Win32\\Debug
                    HelloWorldTests.exe
                    '''
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

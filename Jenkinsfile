pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                echo 'Code already checked out from Git'
            }
        }

        stage('Compile Java') {
            steps {
                bat '''
                rmdir /s /q build 2>nul
                mkdir build
                javac -d build src\\Hello.java
                jar cfe hello.jar Hello -C build .
                '''
            }
        }

        stage('Prepare Package Directory') {
            steps {
                bat '''
                rmdir /s /q package 2>nul
                mkdir package\\bin
                copy hello.jar package\\bin\\hello.jar
                '''
            }
        }

        stage('Build Windows Package (ZIP)') {
            steps {
                bat '''
                powershell Compress-Archive -Force package hello-java-%BUILD_NUMBER%.zip
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: '*.zip'
        }
    }
}

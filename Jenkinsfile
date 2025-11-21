pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    bat 'gradlew.bat clean test'
                    }
                }
            }

        stage('Archive Test Results') {
            steps {
                junit 'build/test-results/test/*.xml'
                archiveArtifacts artifacts: 'build/allure-results/**', fingerprint: true
            }
        }

        stage('Allure Report') {
            steps {
                allure includeProperties: false,
                       jdk: '',
                       results: [[path: 'build/allure-results']]
            }
        }

        stage('Deploy branch') {
            when {
                branch 'GradleAPItask'
            }
            steps {
                echo 'branch GradleAPItask detected, deploying application...'
            }
        }
    }

    post {
        always {
            echo "Build finished with status: ${currentBuild.currentResult}"
        }
    }
}



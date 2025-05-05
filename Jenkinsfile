pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rgoel956/8.1c_ppiit.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('Security Scan (npm audit)') {
            steps {
                sh 'npm audit || true'
            }
        }

stage('SonarCloud Analysis') {
    steps {
        withSonarQubeEnv('SonarScanner') {
            sh '''
                /opt/sonar-scanner/bin/sonar-scanner \
                -Dsonar.login=$SONAR_TOKEN \
                -Dsonar.projectKey=8.1c_ppiit \
                -Dsonar.organization=rgoel956 \
                -Dsonar.sources=. \
                -Dsonar.exclusions=node_modules/**,coverage/** \
                -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
            '''
        }
    }
}




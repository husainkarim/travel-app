pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'Java-21'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Initialize GitHub Status') {
            steps {
                withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                    sh """
                        curl -H "Authorization: token ${TOKEN}" \
                             -H "Content-Type: application/json" \
                             -X POST \
                             -d '{"state": "pending", "context": "Jenkins CI/SafeZone", "description": "Build is in progress...", "target_url": "${env.BUILD_URL}"}' \
                             https://api.github.com/repos/husainkarim/travel-app/statuses/${env.GIT_COMMIT}
                    """
                }
            }
        }

        // Other stages like Build, Test, etc. would go here
        stage('SonarQube Analysis') {
            steps {
                // 'SonarQube' must match the name you give in Jenkins Global Configuration
                withSonarQubeEnv('SonarQube') { 
                    script {
                        // 1. Backend Microservices Analysis

                        // 2. Frontend Analysis
                        dir('frontend') {    
                            sh "npx sonar-scanner \
                                -Dsonar.projectKey=travel-app-frontend \
                                -Dsonar.sources=src \
                                -Dsonar.javascript.lcov.reportPaths=coverage/frontend/lcov.info \
                                -Dsonar.typescript.lcov.reportPaths=coverage/frontend/lcov.info \
                                -Dsonar.host.url=${SONAR_HOST_URL} \
                                -Dsonar.login=${SONAR_AUTH_TOKEN}"
                        }
                    }
                }
            }
        }

        // This stage will wait for SonarQube to process the analysis and return the quality gate status via webhook
        stage("Quality Gate") {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    // Jenkins will pause here until SonarQube sends a webhook back
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Good: Prevents disk space issues and "dirty" builds
        }
        success {
            withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                sh """
                    curl -H "Authorization: token ${TOKEN}" \
                         -H "Content-Type: application/json" \
                         -X POST \
                         -d '{"state": "success", "context": "Jenkins CI/SafeZone", "description": "Build Succeeded!", "target_url": "${env.BUILD_URL}"}' \
                         https://api.github.com/repos/husainkarim/travel-app/statuses/${env.GIT_COMMIT}
                """
            }
            
            mail to: 'husain.akarim@gmail.com',
                 subject: "SUCCESS: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "Great news! The build passed all quality checks. Review it here: ${env.BUILD_URL}"
        }
        failure {
            withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                sh """
                    curl -H "Authorization: token ${TOKEN}" \
                         -H "Content-Type: application/json" \
                         -X POST \
                         -d '{"state": "failure", "context": "Jenkins CI/SafeZone", "description": "Build Failed!", "target_url": "${env.BUILD_URL}"}' \
                         https://api.github.com/repos/husainkarim/travel-app/statuses/${env.GIT_COMMIT}
                """
            }
            
            mail to: 'husain.akarim@gmail.com',
                 subject: "FAILURE: ${env.JOB_NAME} [${env.BUILD_NUMBER}]",
                 body: "The build or security scan failed. Please check the logs immediately: ${env.BUILD_URL}"
        }
    }
}
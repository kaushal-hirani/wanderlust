pipeline {
    agent any
    environment {
        SONAR_HOME = tool "Sonar"
    }
    stages {
        stage("Clone Code from GitHub") {
            steps {
                git url: "https://github.com/kaushal-hirani/wanderlust.git", branch: "devops"
            }
        }
        stage("SonarQube Quality Analysis") {
            steps {
                withSonarQubeEnv("Sonar") {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
        stage("OWASP Dependency Check") {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage("Sonar Quality Gate Scan") {
            steps {
                timeout(time: 2, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System Scan") {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Deploy using Docker compose") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/*-report.*', allowEmptyArchive: true
        }
        success {
            emailext(
                to: 'kaushalhirani99@gmail.com',
                subject: "Jenkins Build Success: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Good news! The Jenkins pipeline has successfully completed.
                - Job Name: ${env.JOB_NAME}
                - Build Number: ${env.BUILD_NUMBER}
                - Build URL: ${env.BUILD_URL}
                Please check the Jenkins dashboard for more details.
                """,
                retry: 5
            )

        }
        failure {
            emailext(
                to: 'kaushalhirani99@gmail.com',
                subject: "Jenkins Build Failure: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: """
                Unfortunately, the Jenkins pipeline failed.

                - Job Name: ${env.JOB_NAME}
                - Build Number: ${env.BUILD_NUMBER}
                - Build URL: ${env.BUILD_URL}

                Please check the console logs for error details.
                """,
                retry: 5
            )
        }
    }
}

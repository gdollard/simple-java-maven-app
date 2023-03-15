pipeline {
    agent {
        docker {
            image 'maven:3.9.0-eclipse-temurin-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Snyk Scanning') {
            steps {
                 snykSecurity(
                    snykInstallation: 'snyk@latest',
                    snykTokenId: 'org-snyk-api-token',
                    // place other optional parameters here, for example:
                    // additionalArguments: '--all-projects --detection-depth=<DEPTH>'
                )
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
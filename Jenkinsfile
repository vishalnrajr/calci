pipeline {
    agent any
    tools {
        jdk 'JDK 17'
        maven 'Maven-3.10'
    }
    stages {
        stage('SCM Synchronization') {
            steps {
                echo 'Fetching source code from GitHub'
                checkout scm
            }
        }
        stage('Static Verification') {
            steps {
                echo 'Validating Maven project'
                bat 'mvn validate'
            }
        }
        stage('Compilation & Automated Testing') {
            steps {
                echo 'Compiling and executing JUnit tests'
                bat 'mvn clean package'
            }
            post {
                always {
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                echo 'Starting SonarQube analysis'
                withSonarQubeEnv('SonarQube-Local') {
                    bat 'mvn sonar:sonar'
                }
            }
        }
    }
    post {
        success {
            echo 'Build and SonarQube analysis completed!'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
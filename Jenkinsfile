pipeline {
    options {timeout(time: "${BUILD_TIMEOUT_MINUTES}", unit: 'MINUTES')}
    environment {
        JAVA_TOOL_OPTIONS = '-Duser.home=/var/maven'
        SONAR_HOST_URL = 'localhost:9000'
    }
    agent any
    tools {
        maven 'Maven 3.6.3'
        jdk 'OpenJDK11'
        //docker
    }
    triggers {
        pollSCM '* * * * *'
    }
    stages {
        /*stage('Tools check') {
            steps {
                sh 'mvn --version'
                sh 'java --version'
                //sh 'docker --version'
            }
        }*/
        stage('Compile & Test') {
            steps {
                sh 'mvn clean install'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('SonarQube Server') {
                    sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar -Dsonar.login=b8892c89ad56dbfede953d27fa9335b6dcdf8699'
                }
            }
        }
    }
}

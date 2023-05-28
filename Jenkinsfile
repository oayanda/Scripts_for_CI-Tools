pipeline {
    agent any
    tools {
        maven "MAVEN3"
        jdk "OraceJDK8"
    }
    stages {
        stage('Fetch code') {
            steps {
                git branch: 'Paac', url: 'https://github.com/oayanda/Scripts_for_CI-Tools'
            }
        }
        stage('Build') {
            steps{
                sh 'mvn install'
            }
            post {
                success {
                    echo 'Now Archiving it.....'
                    archiveArtifacts '**/target/*.war'
                }
            }

        }
        
        
        stage('UNIT TEST') {
            steps {
                sh 'mvn test'
            }

        }
        stage('Checkstyle Analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Sonar Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps {
                withSonarQubeEnv('sonar'){
                    sh ''' ${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile\
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                    
                }
            }
        }
        stage('Quality Gate') {
            steps {
                   timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                }
            }
        }
      
        
    }
}
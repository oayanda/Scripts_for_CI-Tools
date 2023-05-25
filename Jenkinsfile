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
    }
}
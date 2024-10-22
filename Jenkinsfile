pipeline {
    agent any
     
    tools {
        maven 'maven'
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/razak88/web-app.git'
            }
        }

        stage('maven build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('code analysis') {
            environment {
                ScannerHome = tool 'sonar'
            }
            steps {
                script {
                    withSonarQubeEnv('sonar') {
                        sh "${ScannerHome}/bin/sonar-scanner -Dsonar.projectKey=donatus-webapp"
                    }
                }
            }
        }

        stage('uploading to nexus') {
            steps {
                nexusArtifactUploader(
                    artifacts: [[
                        artifactId: 'maven-web-application', 
                        classifier: '', 
                        file: '/var/lib/jenkins/workspace/jomacs-webapp-pipeline/target/web-app.war', 
                        type: 'war'
                    ]], 
                    credentialsId: 'nexus-credentials', 
                    groupId: 'com.mt', 
                    nexusUrl: '44.203.72.44:8081/repository/jomacs-webapp', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'jomac-webapp', 
                    version: '3.1.2-SNAPSHOT'
                )
            }
        }

        stage('prod deployment') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '', url: 'http://54.158.220.239:8080')], 
                contextPath: null, 
                war: 'target/web-app.war'
            }
        }
    }
}

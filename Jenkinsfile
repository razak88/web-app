pipeline{
   agent any
     
    tools{
      maven 'maven'
    }

    stages{
      stage('git checkout'){
        steps{
          git branch: 'main', url: 'https://github.com/razak88/web-app.git'
        }

      }

      stage('maven build'){
        steps{
          sh 'mvn clean package'
        }
      }

      stage('code analysis'){
           environment {
               ScannerHome = tool 'sonar'
           }
           steps{
               script{
                   withSonarQubeEnv('sonar'){
                       sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=donatus-webapp"
                   }
               }
           }
       }

       stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('uploading to nexus'){
          steps{
            nexusArtifactUploader artifacts: [[artifactId: 'maven-web-application', classifier: '', file: '/var/lib/jenkins/workspace/jomacs-webapp-pipeline/target/web-app.war', type: 'war']], credentialsId: 'nexus-credentials', groupId: 'com.mt', nexusUrl: '44.203.72.44:8081/repo/jomacs-webapp', nexusVersion: 'nexus3', protocol: 'http', repository: 'jomac-webapp', version: '3.1.2-SNAPSHOT'
          }
        }
    }

}
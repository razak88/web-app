pipeline{
   agent any
     
    tools{
      maven 'maven'
    }

    stages{
      stage('git checkout'){
        git branch: 'main', url: 'https://github.com/razak88/web-app.git'

      }
    }

}
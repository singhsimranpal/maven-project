pipeline {
    agent any
    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        //Deploy to Staging step after .war file has been created
        stage ("Deploy to Staging"){
            steps {
                build job: 'deploy-to-staging'
            }
        }
    }
}

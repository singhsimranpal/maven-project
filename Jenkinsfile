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

        //Deploy to Production 
        stage ('Deploy to Production'){
            steps{
                //This means that this step will fail if nobody approves it within 5 days
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'//, Submitter: xxx (if allowing specific persom to approve this )
                }

                build job: 'Deploy-to-Prod'
            }

            //If nobody approves within time allotted above, it will fail and display failure message.
            //If deployment is successful, it will dispaly success message
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}

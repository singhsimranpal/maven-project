pipeline {
    agent any

    //Public IP addresses set up for my 2 EC2 Instances (VMs) set up in AWS
    //Best Practice:  Use "parameters" instead of hard coding IP values within scripts (similar to abstracting test cases)
    parameters {
         string(name: 'tomcat_dev', defaultValue: '172-31-91-182', description: 'Staging Server')
         //string(name: 'tomcat_prod', defaultValue: '54.144.41.225', description: 'Production Server')
    }

    //Trigger set for build to run at a specific time (CRON Syntax)
    triggers {
         pollSCM('* * * * *')
     }

    stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            //"Parallel" tells Jenkins to run both the stages below at he same time.  Another example:  Build code and run Checkstyle at the same time
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        //TODO:  Update path to where .pem file is saved
                        //TODO:  If you are running Jenkins on a Windows machine and are not using tools such as Cmder, you will have to change "sh" to "bat" in this script
                        sh "scp -i C:\Users\ssingh\tomcat-demo.pem **\target\*.war ec2-user@${params.tomcat_dev}:\var\lib\tomcat7\webapps"
                    }
                }

                /*stage ("Deploy to Production"){
                    steps {
                        //TODO:  Update path to where .pem file is saved
                        //TODO:  If you are running Jenkins on a Windows machine and are not using tools such as Cmder, you will have to change "sh" to "bat" in this script
                        bat "scp -i C:\Users\ssingh\tomcat-demo.pem **\*.war ec2-user@${params.tomcat_prod}:/varlib/tomcat7/webapps"
                    }
                    */
                }
            }
        }
    }
}

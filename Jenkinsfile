pipeline {
    agent any
    stages {
        stage ('Packer_Servlet_Pipeline') {
            steps {
                /*For windows machine */
               bat  'mvn clean package'

                /*For Mac & Linux machine */
               // sh  'mvn clean package'
            }

            post{
                success{
                    echo 'Now Archiving ....'

                    archiveArtifacts artifacts : '**/*.war'
                }
            }
        }

        stage ('DeployStagingPipeline'){
            steps{

                build job : 'Deploy_Servlet_Project'

            }
        }

        stage ('Deploy_ProdPipeline'){
            steps{
                timeout (time: 5, unit:'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                
                build job : 'Deploy_Prod'
            }

            post{
                success{
                    echo 'Deployment on PRODUCTION is Successful'
                }

                failure{
                    echo 'Deployement Failure on PRODUCTION'
                }
            }
        }
    }
}

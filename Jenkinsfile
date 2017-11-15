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
            stage('Deploy to Staging'){
                steps{
                    build job: 'MvnPrj-deploy-to-staging'
                }
            }

            stage('Deploy to Production'){
                steps{
                    timeout(time:5, unit:'DAYS'){
                        input message: 'Approve PROD Deployment?'
                    }

                    build job: 'MvnPrj-deploy-to-prod'
                }
                post{
                    success {
                        echo 'Code deployed to Prodution.'
                    }

                    failure {
                        echo 'Deployment failed.'
                    }
                }
            }        
        }
}
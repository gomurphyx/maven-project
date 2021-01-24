pipeline {
    agent any
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
        stage ('Deploy to Dev'){
            steps {
                build job: 'Deploy-to-dev'
            }
        }
        
        stage ('Deploy to Stage'){
            steps{
                timeout(time:1, unit:'DAYS'){
                    input message:'Approve Staging Deployment?'
                }

                build job: 'Deploy-to-Stage'
            }
            post {
                success {
                    echo 'Code deployed to Stage.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
        
    }
}

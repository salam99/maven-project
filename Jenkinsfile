 pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.217.125.2', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.219.94.229', description: 'Production Server')
    }

    triggers {
         pollSCM('H/5 * * * *')
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
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /Users/msalam/.ssh/id_rsa **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /Users/msalam/.ssh/id_rsa **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}

 

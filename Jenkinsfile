pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.223.116.74', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '3.19.61.139', description: 'Production Server')
    }

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
            parallel{

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /home/dennis/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
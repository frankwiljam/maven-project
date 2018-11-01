pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'localhost:8080', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.14.154.226', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        bat "copy /Y **/target/*.war C:\\Program Files\\Apache\\apache-tomcat-9.0.12-stage\\webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i C:\\Users\\flofgrens\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
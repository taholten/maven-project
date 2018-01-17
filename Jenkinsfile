pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.218.116.218', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.221.106.167', description: 'Production Server')
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
                        bat 'copy -i "E:\\Dropbox\\aws\\keys\\tomcat-demo.pem" "**\\target\\*.war" "ec2-user@${params.tomcat_dev}:\\var\\lib\\tomcat7\\webapps"'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat 'copy -i "E:\\Dropbox\\aws\\keys\\tomcat-demo.pem" "**\\target\\*.war" "ec2-user@${params.tomcat_prod}:\\var\\lib\\tomcat7\\webapps\"'
                    }
                }
            }
        }
    }
}
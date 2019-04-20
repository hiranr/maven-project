pipeline {
    agent any
    tools {
        maven 'localmaven'
    }
    parameters {
         string(name: 'jenkins-tomcat', defaultValue: 'localhost', description: 'Staging Server')
         string(name: 'jenkins-tomcat2', defaultValue: 'localhost', description: 'Production Server')
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
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp **/target/*.war hiran@${params.jenkins-tomcat}/home/hiran/jenkins-tomcat/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp **/target/*.war hiran@${params.jenkins-tomcat2}/home/hiran/jenkins-tomcat2/webapps"
                    }
                }
            }
        }
    }
}

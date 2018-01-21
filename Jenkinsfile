pipe
line {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    }

stages{
        stage('Build'){
            steps {
                sh 'cat pom.xml'
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.zip'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        echo " deploy done"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo " deploy done"
                    }
                }
            }
        }
    }
}

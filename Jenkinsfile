pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '35.166.210.154', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '34.209.233.6', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
    stage('PreBuild') {
        steps {
            echo 'Getting information for pom file'
            script {
                 def pom = readMavenPom file: 'pom.xml'
                 def versionList = pom.version.replace("-SNAPSHOT", "").tokenize(".")
                 def newRelease = Eval.me(versionList[2])+1
                 def version = "${versionList[0]}.${versionList[1]}.${versionList[2]}"
                 def artifactId = pom.artifactId
                 echo "artifact id " 
                 echo artifactId
                }
        }
    }
      stage('Build'){
            steps {
                sh("""
                    sed -i -e "s/\\/extra\\/empty\\.properties/\\/extra\\/stage\\.properties/g" src/main/app/log4j.xml
                    cat src/main/app/log4j.xml
                    """)
                echo "ARTIFACT ID"
                echo $artifactId
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
                        echo ' deploy done to staging'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        echo 'deploy done to production'
                    }
                }
            }
        }
    }
}

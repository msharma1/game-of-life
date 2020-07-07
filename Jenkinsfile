pipeline {
   agent any

   tools {
      // Install the Maven version configured as "M3" and add it to the path.
      maven "myMorningMaven"
   }

   stages {
      stage('parallel-1'){
       parallel{
        stage ('yumUpdateInstall'){
          steps{
            sh "sudo yum update -y"
            sh "sudo yum install git -y"
          }
        }
       }
      }
      stage ('createUser'){
         steps{
             sh "sudo adduser ${myUser}"
         }
      }
      stage('Build') {
        input{
        message "Continue?"
        ok "yes"
        submitter "msharma1,jenkins"
     parameters{
       string(name: "echoMaterial", defaultValue: "defaultValueForEchoMateria", description: "This is the demo for param")
     }
   }
    options{
      timeout(time: 1,unit: 'HOURS')
    }
    steps {
            sh "echo Printing ${params.echoMaterial}"
            sh "sleep 15"
            // Get some code from a GitHub repository
            git 'https://github.com/msharma1/game-of-life.git'

            // Run Maven on a Unix agent.
            sh "mvn -Dmaven.test.failure.ignore=true clean package"

            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
         }

         post {
            // If Maven was able to run the tests, even if some of the test
            // failed, record the test results and archive the jar file.
            success {
               junit '**/target/surefire-reports/TEST-*.xml'
               archiveArtifacts '**/target/*'
            }
         }
      }
     
   }
}

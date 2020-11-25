pipeline{
    agent{
        label 'jenkins_slave'
    }
   parameters {
      choice choices: ['dv', 'qa', 'qc', 'prod'], description: '', name: 'env'
      string defaultValue: 'names', description: 'vasmi', name: 'names', trim: false
    }
    triggers {
        pollSCM 'H/20 * * * *'
     }
     stages{
         stage("build"){
             steps{
              git changelog: false, credentialsId: '747ab094-b6c7-4a84-bf17-802b3936f8cb', url: 'https://github.com/vamsi0047/hello-world.git'
             }
         }
         stage("test"){
             steps{
               sh "mvn clean test"
             }
         }
         stage("package"){
             steps{
                sh "mvn package"
             }
         }
     }
     post{
        success {
            archive '**/webapp/target/*.war'
             junit healthScaleFactor: 0.1, skipPublishingChecks: true, testResults: '**/target/**/*.xml'
            archive '**/target/**/*.xml'
          }
        failure{
            echo "build failed"
        }
     }
}

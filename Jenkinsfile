// pipeline
// {
//   agent any
  
//   tools
//   {
//     maven 'Maven_3.8.2'
//   }
  
//   triggers
//   {
//     pollSCM('* * * * *')
//   }
  
//   options
//   {
//     timestamps()
//     buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '5', daysToKeepStr: '', numToKeepStr: '5'))
//   }
  
//   stages
//   {
//     stage('Checkout Code from GitHub')
//     {
//       steps()
//       {
//         git branch: 'development', credentialsId: '957b543e-6f77-4cef-9aec-82e9b0230975', url: 'https://github.com/devopstrainingblr/maven-web-application-1.git'
//       }
//     }
    
//     stage('Build Project')
//     {
//       steps()
//       {
//         sh "mvn clean package"
//       }
//     }
    
//     stage('Execute SonarQube Report')
//     {
//       steps()
//       {
//         sh "mvn clean sonar:sonar"
//       }
//     }
    
//     stage('Upload Artifacts to Sonatype Nexus')
//     {
//       steps()
//       {
//         sh "mvn clean deploy"
//       }
//     }
    
//     stage('Deploy Application to Tomcat')
//     {
//       steps()
//       {
//         sshagent(['bfe1b3c1-c29b-4a4d-b97a-c068b7748cd0'])
//         {
//           sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.190.162:/opt/apache-tomcat-9.0.50/webapps/"
//         }
//       }
//     }
//   }

// post
// {
//   success
//   {
//     emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
//     subject: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
//     body: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
//     replyTo: 'devopstrainingblr@gmail.com'
//   }
//   failure
//   {
//     emailext to: 'devopstrainingblr@gmail.com,mithuntechnologies@yahoo.com',
//     subject: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
//     body: "Pipeline Build is Over Build # is ${env.BUILD_NUMBER} and Build Status is ${currentBuild.result}",
//     replyTo: 'devopstrainingblr@gmail.com'
//     }
//   }
// }


node {
    def mavenHome=tool name:"maven_new"
    stage('checkout'){
        git 'https://github.com/hariikrishn/maven-web-application.git'
    }
    stage('Build'){
        sh "$mavenHome/bin/mvn clean package"
    }
    stage('uploadtonexus'){
        sh "$mavenHome/bin/mvn deploy"
    }
    stage('deploytotomcat'){
         sshagent(['28b0aacb-ed6f-4f24-81d5-194688b46dc9']) {
            sh """
                scp -o StrictHostKeyChecking=no target/maven-web-application.war ubuntu@3.110.143.115:/tmp/maven-web-application.war
                ssh ubuntu@3.110.143.115 "sudo mv /tmp/maven-web-application.war /opt/tomcat/apache-tomcat-10.1.46/webapps/"
               """
         }
    }
}

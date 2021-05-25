pipeline{
agent any

  tools{
  maven 'maven3.8.1'
  }
  triggers{
  //pollscm triggering
  pollSCM('* * * * *')
  }
  stages{
   stage( 'CheckoutCode'){
     steps{
      git branch: 'development', credentialsId: '39456124-dda4-426d-b95a-5977f6e96492', url: 'https://github.com/rocknrolab/maven-web-application.git'
     }
   }

   stage( 'Build'){
    steps{
    sh "mvn clean package"
    }
   }

   stage( 'ExecuteSonar'){
    steps{
    sh "mvn clean sonar:sonar"
    }
   }

   stage( 'UploadAritifacts'){
    steps{
    sh "mvn clean deploy"
    }
   }

   stage( 'Deploy'){
    steps{
    sshagent(['f616e0b2-147a-4a08-be26-18695aeddcd0']) {
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.235.9.157:/opt/apache-tomcat-9.0.46/webapps/"
}
    }
   }
     

   
  }

  post{
   success{
   emailext body: '''build over..success

Regards
Anil Soni
DevopsEng''', subject: 'build over', to: 'introspector007@gmail.com'
   }
   failure{
   emailext body: '''build over..

Regards
Anil Soni
DevopsEng''', subject: 'build over', to: 'introspector007@gmail.com'
   
   }
  }


}

pipeline { 

agent any
  tools {
  maven 'Maven'
  }
  stages{
    stage ('Initialize'){
      steps {
      sh '''
      echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    '''
      }
    }
    
     stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/satyam-srivastava1997/devsecops.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    
    stage ('Build') {
      steps{
    sh 'mvn clean package'
    }
  }
    
     stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.202.234.129:/prod/apache-tomcat-8.5.69/webapps/webapp.war'
              }      
           }       
    }
    
  }
}

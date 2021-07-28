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
    
    
  
 
    
    
    
    
    
    stage ('Build') {
      steps{
    sh 'mvn clean package'
    }
  }
    
     stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.237.110.166:/prod/apache-tomcat-8.5.69/webapps/webapp.war'
              }      
           }       
    }
    
     stage ('DAST') {
      steps {
        sshagent(['ZAP']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@18.207.206.187 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://54.237.110.166:8080/webapp/" || true'
        }
      }
    }
    
    
    
  }
}

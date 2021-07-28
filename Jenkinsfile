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
    
    
    
    stage ('Source Composition Analysis') {
      steps {
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/satyam-srivastava1997/devsecops/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
        
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@34.239.253.226:/prod/apache-tomcat-8.5.69/webapps/webapp.war'
              }      
           }       
    }
    
  }
}

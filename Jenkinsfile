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
        sh 'docker run gesellix/trufflehog --json https://github.com/cehkunal/webapp.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    
     stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
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
                sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@35.153.98.198:/prod/apache-tomcat-8.5.69/webapps/webapp.war'
              }      
           }       
    }
    
     stage ('DAST') {
      steps {
        sshagent(['ZAP']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@54.209.147.161 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://35.153.98.198:8080/webapp/" || true'
        }
      }
    }
    
    
    
  }
}

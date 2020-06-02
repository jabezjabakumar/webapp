pipeline {
    agent any 
  tools {
   maven 'Maven'
   }
  stages {
    stage ('Initialize') {
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
        sh 'docker run gesellix/trufflehog --json https://github.com/jabezjabakumar/webapp.git > trufflehog'
        sh 'cat trufflehog'
     }
   }      
      stage ('Build') {
         steps {
         sh 'mvn clean package'
                }
      }
      stage ('DAST') {
      steps {
        sshagent(['zap']) {
         sh 'ssh -o  StrictHostKeyChecking=no ubuntu@13.233.168.57 "docker run -t owasp/zap2docker-stable zap-baseline.py -t http://13.127.63.73:8080/webapp/" || true'
        }
      }
    }
    
    }
}

pipeline {
  agent any
  parameters {
    string(name: 'tomcat_dev', defaultValue: 'tomcat-qa', description: 'Staging server')
    string(name: 'tomcat_prod', defaultValue: 'tomcat-prod', description: 'Production server')
  }

  triggers {
    pollSCM('* * * * *')
  }
  
  stages{
    stage('Build') {
      steps {
        sh 'mvn clean package'
      }
    }
    stage('Deployments') {
      parallel {
        stage('Deploy to staging') {
          steps {
            sh "scp -i /var/lib/jenkins/.ssh/id_rsa **/target/*.war root@${params.tomcat_dev}:/usr/local/tomcat/webapps"
          }
        }
        stage('Deploy to production') {
          steps {
            sh "scp -i /var/lib/jenkins/.ssh/id_rsa **/target/*.war root@${params.tomcat_prod}:/usr/local/tomcat/webapps"
          }
        }
      }
    }
  }
}

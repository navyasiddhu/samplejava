pipeline {
  agent none
  stages {
    stage('Build and Test') {
      agent {
        node {
          label 'dockerimage'
        }

      }
      steps {
        sh '''echo "starting the Maven clean package process"
mvn -Dmaven.text.failure.ignore clean package
echo "end of the maven clean command"
'''
        stash(name: 'build-test-artificats', includes: '**/target/surefire-reports/TEST-*.xml, target/*.jar')
      }
    }

  }
}
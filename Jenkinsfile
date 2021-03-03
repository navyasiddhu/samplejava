pipeline {
  agent {
    node {
      label 'dockerimage'
    }

  }
  stages {
    stage('Build and Test') {
      agent {
        node {
          label 'dockerimage'
        }

      }
      steps {
        sh '''echo "starting the Maven clean package process"
mvn -Dmaven.test.failure.ignore clean package
echo "end of the maven clean command"
'''
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml, target/*.jar')
      }
    }

    stage('Report and Publish') {
      agent {
        node {
          label 'dockerimage'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit '**/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }

  }
}
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
mvn -Dmaven.test.failure.ignore clean package
echo "end of the maven clean command"
'''
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml, target/*.jar')
      }
    }

    stage('Report and Publish') {
      parallel {
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

        stage('Publish to Artifactory') {
          agent {
            node {
              label 'dockerimage'
            }

          }
          steps {
            script {
              unstash 'build-test-artifacts'

              def server = Artifactory.server 'ArtifactoryServer'
              def uploadSpec = """{
                "files": [
                  {
                    "pattern": "target/*.jar",
                    "target": "example-repo-local/${BRANCH_NAME}/${BUILD_NUMBER}/"
                  },
                  {
                    "pattern": "**/target/surefire-reports/TEST-*.xml"
                    "target": "example-repo-local/${BRANCH_NAME}/${BUILD_NUMBER}/"
                  }
                ]

              }"""

              server.upload(uploadSpec)
            }

          }
        }

      }
    }

  }
}
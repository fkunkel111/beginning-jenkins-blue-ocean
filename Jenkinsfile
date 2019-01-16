pipeline {
  agent none
  stages {
    stage('Build & Test') {
      agent {
        node {
          label 'docker_ssh'
        }

      }
      steps {
        dir(path: './Ch03/example-maven-project')
        sh 'mvn -Dmaven.test.failure.ignore clean package'
        stash(name: 'build-test-artifacts', includes: '**/target/surefire-reports/TEST-*.xml,target/*.jar')
      }
    }
    stage('Report & Publish') {
      agent {
        node {
          label 'docker_ssh'
        }

      }
      steps {
        unstash 'build-test-artifacts'
        junit ' **/target/surefire-reports/TEST-*.xml'
        archiveArtifacts(artifacts: 'target/*.jar', onlyIfSuccessful: true)
      }
    }
  }
}
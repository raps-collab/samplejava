def artifactName = "ppm-jar" // also used for nexus filePath and artifactId attributes
def artifactVersion = "5.${env.BUILD_NUMBER}"
def artifactSemVersion = "${artifactVersion}.0"
def repoName = "ppm-repo"
pipeline {
  agent any
  stages {
    stage('CI') {
      steps {
        echo 'running CI'
      }
    }

    stage('TestSonarChanges') {
      steps {
        echo 'TestSonarChanges'
      }
    }

    stage('UAT deploy') {
      steps {
        snDevOpsChange()
        echo 'UAT deploy'
      }
    }

    stage('UAT test') {
      steps {
        kube 'runnig UAT deploy'
        echo 'UAT test'
      }
    }

  }
}
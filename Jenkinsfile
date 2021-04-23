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
              sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn compile'
              sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn verify'
        	}
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
        }
        stage('UAT deploy') {
            steps {
		            echo 'running UAT deploy'
                sh '/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/Maven/bin/mvn package'
		            snDevOpsArtifact(artifactsPayload:"""{"artifacts": [{"name": "${artifactName}","version":"${artifactVersion}","semanticVersion": "${artifactSemVersion}","repositoryName": "${repoName}"}]}""")
	    	   }
        }
        stage('UAT test') {
          	steps {
                echo 'running UAT deploy'
                snDevOpsChange()
		        }
        }
	}
}

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
              sh '/usr/local/bin/mvn compile'
              sh '/usr/local/bin/mvn verify'
        	}
            post {
                always {
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
        }
       
	stage('TestSonarChanges') {
	    environment {
    		 SCANNER_HOME = tool 'sonarScanner'
   		 //ORGANIZATION = "igorstojanovski-github"
    		 //PROJECT_NAME = "igorstojanovski_jenkins-pipeline-as-code"
  		}
	      steps {
		echo 'TestSonarChanges'
		      //sh 'def scannerHome = tool 'SonarScanner 4.0';'
		       withSonarQubeEnv('SonarQube_Cloud') {
			     //  sh “/usr/local/apache-maven/apache-maven-3.3.9/bin/mvn sonar:sonar -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=admin -Dsonar.password=admin -Dsonar.github.repository=bvelivala/SampleInsurance -Dsonar.projectName=Autoclaim_${BUILD_NUMBER} -Dsonar.projectVersion=${BUILD_NUMBER} -Dsonar.sources=src/main”
			     // sh 'sonarScanner -Dproject.settings=sonar-scanner.properties'
			      // sh './gradlew sonarqube'
			      // println ${env.SONAR_HOST_URL}
			       sh '${SCANNER_HOME}/bin/sonar-scanner -Dproject.settings=${SCANNER_HOME}/conf/sonar-scanner.properties'

		}
	      }
    	}
        stage('UAT deploy') {
            steps {
		            echo 'running UAT deploy'
                sh '/usr/local/bin/mvn package'
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
        post {
		always {
		    archiveArtifacts artifacts: 'build/libs/**/*.jar', fingerprint: true
		    junit 'build/reports/**/*.xml'
		}
        }
}

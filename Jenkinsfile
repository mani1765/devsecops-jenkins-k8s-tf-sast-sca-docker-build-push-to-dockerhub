pipeline {
  agent any
  tools { 
        maven 'maven3'  
    }
   stages{
    stage('CompileandRunSonarAnalysis') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=easybuggyapp01mk -Dsonar.organization=easybuggyapp01mk -Dsonar.host.url=https://sonarcloud.io -Dsonar.token=f0f336af75a237f0103af1dc4b84c5cdb7c9aeb0'
			}
    }

	stage('RunSCAAnalysisUsingSnyk') {
            steps {		
				withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
					sh 'mvn snyk:test -fn'
				}
			}
    }

	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app = docker.build("asg")
	         
                 }
               }
            }
    }
	stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://677554969074.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:aws-creds') {
                    app.push("latest")
                    }
                }
            }
    	}

	
	    
  }
}

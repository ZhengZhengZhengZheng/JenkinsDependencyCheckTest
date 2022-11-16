pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git 'https://github.com/ZhengZhengZhengZheng/JenkinsDependencyCheckTest.git'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML --suppression suppression.xml', odcInstallation: 'Default'
			}
		}
		
		stage('Code Quality Check via SonarQube') {
			 steps {
			 	script {
			 		def scannerHome = tool 'SonarQube';
			 		withSonarQubeEnv('SonarQube') {
			 		sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=DependencyCheckTest -Dsonar.sources=."
			 		}
			 	}
			 }
		}

	}	
	post {
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
		always {
 			recordIssues enabledForFailure: true, tool: sonarQube()
 		}

	}
}

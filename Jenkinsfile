pipeline {
	agent any

	
	environment {
		IMAGE_NAME = "sailaja868/trivy-scan:latest"
		// DOCKER_HOST ='/usr/local/bin'
		TRIVY_HOST  ='/etc/apt/sources.list.d'
		}
	
	// aquasec/trivy
	options{
		buildDiscarder(logRotator( artifactDaysToKeepStr: "${env.BRANCH_NAME}" == 'main'? '5': "${env.BRANCH_NAME}".startsWith("release")? '3' : '1', 
            daysToKeepStr: "${env.BRANCH_NAME}" == 'main' ? '5': "${env.BRANCH_NAME}".startsWith("release")? '3':'1'
        ))
	}
	stages {
		// stage('initialize') {
		// 	steps {
				
		// 		sh	"${DOCKER_HOST}/docker version" // DOCKER_CERT_PATH is automatically picked up by the Docker client
		// 	}
		// }
		stage("Scan") {

			steps {
				sh '${TRIVY_HOST}/trivy image ${IMAGE_NAME} > scan.txt'
			}
		}
	}
	post {
        always {
            echo 'Sending email notification'
             sh 'cat scan.txt'
			    
                emailext attachmentsPattern: 'scan.txt',
                to: 'psailu868@execs.com',
				from: "Jenkins",
				body: 'Here is the Scan report for Trivy',
                subject: "Trivy Scan Report"
            
        }
    }

	
}

node {
 	// Clean workspace before doing anything
    deleteDir()

    try {
        stage ('Clone') {
        	checkout scm
        }
        stage ('install dependencies') {
        	sh "echo 'shell scripts to build project...'"
        }
        stage ('Tests') {
	        parallel 'static': {
	            sh "echo 'shell scripts to run static tests...'"
	        },
	        'unit': {
	            sh "echo 'shell scripts to run unit tests...'"
	        },
	        'integration': {
	            sh "echo 'shell scripts to run integration tests...'"
	        }
        }

        stage('Deploy if dev') {
            when {
            expression {
                return env.BRANCH_NAME != 'uat';
                }
            }
            steps {
                echo 'run this stage - when branch is not equal to uat'
            }
        }
        
      	stage ('Deploy') {
            sh "echo 'shell scripts to deploy to server...'"
      	}
    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}

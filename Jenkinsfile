node {
 	// Clean workspace before doing anything
    deleteDir()

    environment {
        ENV_NAME = "${env.BRANCH_NAME}"
        USER_NAME = ""
        DEV_SERVER = "dev_server.com"
        UAT_SERVER = "uat_server.com"
        SIT_SERVER = "sit_server.com"
        PROD_SERVER = "prod_server.com"
        DEPLOYMENT_TARGET = ""
    }

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
          script {
            if (ENVIRONMENT_NAME == 'dev') {
                  DEPLOYMENT_TARGET = DEV_SERVER
            } else if (ENVIRONMENT_NAME == 'uat') {
              DEPLOYMENT_TARGET = UAT_SERVER
            }else if (ENVIRONMENT_NAME == 'sit') {
              DEPLOYMENT_TARGET = SIT_SERVER
            }else if (ENVIRONMENT_NAME == 'prod') {
              DEPLOYMENT_TARGET = PROD_SERVER
            }
          }
              sh 'ssh '${USER_NAME}'@'${DEPLOYMENT_TARGET}' rm -rf /tmp/deployment/dist/'
              sh 'ssh '${USER_NAME}'@'${DEPLOYMENT_TARGET}' mkdir -p /tmp/deployment/'
              sh 'scp -r dist '${USER_NAME}'@'${DEPLOYMENT_TARGET}':/tmp/deployment/'
              sh 'ssh '${USER_NAME}'@'${DEPLOYMENT_TARGET}' “rm -rf /var/www/example.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/example.com/”'
        }

    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}

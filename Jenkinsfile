def INPUT_PARAMS = "test here yoh!!"
def ENVIRONMENT_NAME = "${env.BRANCH_NAME}"
def USER_NAME = ""
def DEV_SERVER = "dev_server.com"
def UAT_SERVER = "uat_server.com"
def SIT_SERVER = "sit_server.com"
def PROD_SERVER = "prod_server.com"
def DEPLOYMENT_TARGET = ""

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

        stage('Do you want to deploy?') {
                script {
                    env.IS_TO_DEPLOY = input message: 'Input required',
                    parameters: [choice(name: 'Deploy', choices: 'no\nyes', description: 'Choose "yes" if you want to deploy this build')]
                  }
                  sh "echo 'messages are ${env.IS_TO_DEPLOY}'"
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

          sh "echo 'Deploying to ${DEPLOYMENT_TARGET} for branch name ${ENVIRONMENT_NAME}'"
        //       sh 'ssh ${USER_NAME}@${DEPLOYMENT_TARGET} rm -rf /tmp/deployment/dist/'
        //       sh 'ssh ${USER_NAME}@${DEPLOYMENT_TARGET} mkdir -p /tmp/deployment/'
        //       sh 'scp -r dist ${USER_NAME}@${DEPLOYMENT_TARGET}:/tmp/deployment/'
        //       sh 'ssh '${USER_NAME}@${DEPLOYMENT_TARGET} “rm -rf /var/www/example.com/dist/ && mv /var/www/temp_deploy/dist/ /var/www/example.com/”'
        }

    } catch (err) {
        currentBuild.result = 'FAILED'
        throw err
    }
}

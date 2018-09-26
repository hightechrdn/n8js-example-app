pipeline {

/*	options {
		timestamps()
	} */

	agent {
		node {
			label 'jenkins-slave-s2i-rhel7' 
		}
	}

	tools {
		oc 'oc3.9'
	} 

/*	parameters {
		choice(choices: "$environment", description: '', name: 'ENVIRONMENT')
		string(defaultValue: "$emailRecipients",
			description: 'List of email recipients',
			name: 'EMAIL_RECIPIENTS')
	} */
    
	environment {
		def TAG = "${GIT_COMMIT:0:7}"
	}

	stages {
		stage ('Checkout'){
			steps {
					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/mcdmike74/n8js-example-app.git']]])
			}
		}

				//		s2i build . 172.30.1.1:5000/jenkins/nodejs8-builder-rhel7 n8js-example-app-builder-test:${env.BUILD_ID} --exclude '(^|/)\\.git(/|\$)|(J|j)enkinsfile'
		stage('Build Image') {
			steps {
				script {
					sh """
						echo hello world
						id; ls -lR; ls -l /var/run/
                        echo ${TAG}
                        env
					"""
				}	// script
			} // steps
		} // stage:Build Image

		stage('Deploy') {
			steps {
				script {
					openshift.withCluster( 'openshift' ) {
						openshift.withProject( 'jenkins' ) {
							echo "${openshift.raw( "version" ).out}"
							echo "In project: ${openshift.project()}" 
						}
					}                            
				} // script
			} // steps
		} // stage:Deploy
		
	} // stages

	post {
		always {
			// clean up workspace
			echo "Did it work?"
			// deleteDir()
		}
		
		success {
			echo "Success!"
		}
	}

} // pipeline

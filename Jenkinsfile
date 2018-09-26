pipeline {

	agent {
		node {
			label 'jenkins-slave-s2i-rhel7' 
		} // node
	} // agent

	tools {
		oc 'oc3.9'
	} 

/*	parameters {
		choice(choices: "$environment", description: '', name: 'ENVIRONMENT')
		string(defaultValue: "$emailRecipients",
			description: 'List of email recipients',
			name: 'EMAIL_RECIPIENTS')
	} */
    
/*	environment {
		VARIABLE = 'test'
	} */

	stages {
		stage ('git'){
			steps {
				checkout([
/*					$class: 'GitSCM',
					branches: scm.branches,
					doGenerateSubmoduleConfigurations: false,
					extensions: scm.extensions + [[$class: 'SubmoduleOption', disableSubmodules: false, recursiveSubmodules: true, reference: '', trackingSubmodules: false]],
					submoduleCfg: [],
					userRemoteConfigs: scm.userRemoteConfigs]) */

					checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/mcdmike74/n8js-example-app.git']]])


//					git 'https://github.com/mcdmike74/n8js-example-app.git'

			}
		}

		stage('Build Image') {
			steps {
				script {
					sh 'echo hello world'
					sh 'id; ls -lR; ls -l /var/run/'
//					sh 's2i build https://github.com/mcdmike74/n8js-example-app  172.30.1.1:5000/jenkins/nodejs8-builder-rhel7 n8js-example-app-builder-test'
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

} // pipeline
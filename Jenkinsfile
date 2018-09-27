import hudson.model.*

node('jenkins-slave-s2i-rhel7') {

	stage('Checkout Source') {
	  checkout scm
	}

	// def env = build.getEnvironment()
	def gitCommit = env['GIT_COMMIT']
	def shortGitCommit = gitCommit[0..7]

	/* def pa = new ParametersAction([
	  new StringParameterValue("SHORT_GIT_COMMIT", shortGitCommit)
	])

	// add variable to current job
	build.addAction(pa) */

	tool name: 'oc3.9', type: 'oc'

	stage('Build Image') {
		sh """
			echo hello world
			id; ls -lR; ls -l /var/run/
		#	s2i build . 172.30.1.1:5000/jenkins/nodejs8-builder-rhel7 n8js-example-app-builder-test:${env.BUILD_ID} --exclude '(^|/)\\.git(/|\$)|(J|j)enkinsfile'
		"""
	} // stage:Build Image

	stage('Deploy') {
		openshift.withCluster( 'openshift' ) {
			openshift.withProject( 'jenkins' ) {
				echo "${openshift.raw( "version" ).out}"
				echo "In project: ${openshift.project()}" 
			}
		}                            
	} // stage:Deploy
		
} // node

import hudson.model.*

node('jenkins-slave-s2i-rhel7') {

echo "mark 0"
	stage('Checkout Source') {
	  checkout scm
	}


echo "mark 1"
	def gitCommit = env['GIT_COMMIT']
	def shortGitCommit = gitCommit[0..7]
    echo ${pa}

echo "mark 2"
	tool name: 'oc3.9', type: 'oc'

echo "mark 3"
	stage('Build Image') {
		sh """
			echo hello world
			id; ls -lR; ls -l /var/run/
echo GIT_SHA_SHORT=`git rev-parse --short=8 ${GIT_COMMIT}`
		#	s2i build . 172.30.1.1:5000/jenkins/nodejs8-builder-rhel7 n8js-example-app-builder-test:${env.BUILD_ID} --exclude '(^|/)\\.git(/|\$)|(J|j)enkinsfile'
		"""
	} // stage:Build Image

echo "mark 4"
	stage('Deploy') {
		openshift.withCluster( 'openshift' ) {
			openshift.withProject( 'jenkins' ) {
				echo "${openshift.raw( "version" ).out}"
				echo "In project: ${openshift.project()}" 
			}
		}                            
	} // stage:Deploy
		
} // node

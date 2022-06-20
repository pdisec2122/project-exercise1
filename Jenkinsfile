node {

	def commit_id

	 def to = emailextrecipients([
		[$class: 'CulpritsRecipientProvider'],
		[$class: 'DevelopersRecipientProvider'],
		[$class: 'RequesterRecipientProvider']
	]);

	try {
		stage('Preparation') {
			git branch: 'main', url: 'https://github.com/pdisec2122/project-exercise1.git'
			sh "git rev-parse --short HEAD > .git/commit-id"
			commit_id = readFile('.git/commit-id').trim()
		}

		stage('docker build/push') {
			docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
				def app = docker.build("pdisec2122/project-exercise1:${commit_id}", '.').push()
			}
		}

		currentBuild.result = "SUCCESS";

	} catch (e) {
		currentBuild.result = "FAILURE";
		throw e;

	} finally {
		def subject = "${env.JOB_NAME} - Build #${env.BUILD_NUMBER} ${currentBuild.result}"
		def content = '${JELLY_SCRIPT,template="html"}'
		
		if (to != null && !to.isEmpty()) {
			emailext(body: content, mimeType: 'text/html',
				replyTo: '$DEFAULT_REPLYTO', subject: subject,
				to: to, attachLog: true )
		}
	}

}

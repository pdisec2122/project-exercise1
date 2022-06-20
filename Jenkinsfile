node {

	def commit_id
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
	
}

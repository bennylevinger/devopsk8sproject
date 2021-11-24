def label = "docker-jenkins-${UUID.randomUUID().toString()}"
def home = "/home/jenkins"
def workspace = "${home}/workspace/build-docker-jenkins"
def workdir = "${workspace}/src/localhost/docker-jenkins/"

def ecrRepoName = "my-jenkins"
def tag = "$ecrRepoName:latest"

podTemplate(label: label,
		containers: [
				containerTemplate(name: 'jnlp', image: 'jenkins/jnlp-slave:alpine'),
				containerTemplate(name: 'consumer', image: 'docker', command: 'cat', ttyEnabled: true),
				containerTemplate(name: 'producer', image: 'docker', command: 'cat', ttyEnabled: true),
		],
		volumes: [
				hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock'),
				hostPathVolume(hostPath: '/home/jenkins/agent/workspace/', mountPath: '/var/poject'),
		],
) {
	node(label) {
		stage('Docker consumer Build') {
			container('consumer') {
				echo "Building consumer docker image..."
			    sh "cd /var/poject && ls -l"
				//sh "cd /var/poject/devopsk8sproject/consumer && docker build"
			}
		}
		stage('Docker producer Build') {
        			container('producer') {
        				echo "Building consumer docker image..."
						sh "cd /var/poject && ls -l"
        				
        				//sh "cd /var/poject/devopsk8sproject/producer && docker build"
        			}
        		}
	}
}
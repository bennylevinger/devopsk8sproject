def label = "docker-jenkins-${UUID.randomUUID().toString()}"
def home = "/home/jenkins"
def workspace = "${home}/workspace/build-docker-jenkins"
def workdir = "${workspace}/src/localhost/docker-jenkins/"
def agentwokspace = "/home/jenkins/agent/workspace/"
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
				//hostPathVolume(hostPath: '/var/jenkins_home/workspace/', mountPath: '/home/jenkins/agent/workspace'),
		],
) {
	node(label) {
	   stage('debug') {
			
				echo "debuging"
			    sh "git clone https://github.com/bennylevinger/devopsk8sproject.git"
				sh "pwd && ls"
			
		}
		stage('Docker consumer Build') {
			container('consumer') {
				echo "Building consumer docker image..."
			    sh "printenv"
				sh "cd devopsk8sproject/consumer && docker build"
			}
		}
		stage('Docker producer Build') {
        			container('producer') {
        				echo "Building consumer docker image..."
						sh "printenv"
        				
        				sh "cd devopsk8sproject/producer && docker build"
        			}
        		}
	}
}
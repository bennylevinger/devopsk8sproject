@Library('jenkins-shared-library') _
def label = "docker-jenkins-${UUID.randomUUID().toString()}"
def home = "/home/jenkins"
def workspace = "${home}/workspace/build-docker-jenkins"
def workdir = "${workspace}/src/localhost/docker-jenkins/"

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
)

 pipeline {
	agent {
            label label
        }
	stages {
	   stage('Git Clone') {
		   steps {
				   echo "Cloning ....."
			       sh "git clone https://github.com/bennylevinger/devopsk8sproject.git"
                 }
		}
		
		stage('Docker consumer Build') {


			   // sh "printenv"
				//sh "cd devopsk8sproject/consumer && docker build ."
			   steps {

			   	echo "Building consumer docker image..."
			   	container('consumer') {
				    def tag = "consumer:1.0.${BUILD_NUMBER}"
                     script {
				      buildDocker(tag,"${workspace}/devopsk8sproject/consumer" , false , "Dokerfile")
				       }
				   }
			    }
		}
		stage('Docker producer Build') {


						//sh "printenv"
        				
        				//sh "cd devopsk8sproject/producer && docker build ."
						   steps {
						   echo "Building consumer docker image..."
					   container('producer') {
                            def tag = "producer:1.0.${BUILD_NUMBER}"
                                     script {
                            buildDocker(tag,"${workspace}/devopsk8sproject/consumer" , false , "Dokerfile")
                            }
						}
        			}
        		}
	}
 
}
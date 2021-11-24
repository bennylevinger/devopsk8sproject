@Library('jenkins-shared-library') _
def label = "docker-jenkins-${UUID.randomUUID().toString()}"
def home = "/home/jenkins"
def workspace = "${home}/workspace/build-docker-jenkins"
def workdir = "${workspace}/src/localhost/docker-jenkins/"


pipeline {
agent {
   kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: jnlp
    image: jenkins/jnlp-slave:alpine
  - name: consumer
    image: docker
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: producer
    image: docker
    command: ['cat']
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
"""
        }
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
			   	 script {
				        def tag = "consumer:1.0.${BUILD_NUMBER}"

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
					   script {
                            def tag = "producer:1.0.${BUILD_NUMBER}"

                            buildDocker(tag,"${workspace}/devopsk8sproject/consumer" , false , "Dokerfile")
                            }
						}
        			}
        		}
	}
 

}
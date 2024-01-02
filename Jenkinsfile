def containerName="insurance"
def tag="latest"
def dockerHubUser="zarrinmujahid"
def gitURL="https://github.com/ZarrinMujahid/star-agile-insurance-project "

node {

    stage('Checkout') {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: gitURL]]]
    }

    stage('Build'){
        sh "mvn clean install"
    }

    stage("Image Prune"){
         sh "docker image prune -f"
    }

    stage('Image Build'){
        sh "docker build -t $containerName:$tag --pull --no-cache ."
        echo "Image build complete"
    }

    stage('Push to Docker Registry'){
        withCredentials([usernamePassword(credentialsId: 'dockerHubAccount', usernameVariable: 'dockerUser', passwordVariable: 'dockerPassword')]) {
            sh "docker login -u $dockerUser -p $dockerPassword"
            sh "docker tag $containerName:$tag $dockerUser/$containerName:$tag"
            sh "docker push $dockerUser/$containerName:$tag"
            echo "Image push complete"
        }
    }
	stage("Kubernetes Deployment"){
		sh "kubectl get pods -o wide"
		sh"kubctl apply -f deployment.yml
		
	}
       /*	stage("Run docker image"){
               sh "docker run -d -p 8082:8082 $containerName:$tag"
	}
	
	stage("Ansible Deploy"){
        ansiblePlaybook inventory: 'hosts', playbook: 'ansible-playbook.yml'
    }*/
}

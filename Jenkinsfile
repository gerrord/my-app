node{
   stage('SCM Checkout'){
     git 'https://github.com/gerrord/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('Build Docker Image'){
   sh 'docker build -t gerrord/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u gerrord -p ${dockerPassword}"
    }
   sh 'docker push gerrord/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p admin123 13.232.168.249:8083"
   sh "docker tag gerrord/myweb:0.0.2 13.232.168.249:8083/gerrord:1.0.0"
   sh 'docker push 13.232.168.249:8083/gerrord:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest gerrord/myweb:0.0.2' 
   }
}
}

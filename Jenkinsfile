node{
   stage('SCM Checkout'){
     git 'https://github.com/ashikali22/my-app.git'
   }
   stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   
   stage('Build Docker Image'){
   sh 'docker build -t saidamo/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u saidamo -p ${dockerPassword}"
    }
   sh 'docker push saidamo/myweb:0.0.2'
   }
 
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest ashikali22/myweb:0.0.2' 
   }
}

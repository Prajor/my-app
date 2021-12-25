node{
   stage('SCM Checkout'){
     git 'https://github.com/Prajor/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t 87545056278072821043/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u 87545056278072821043 -p ${dockerPassword}"
    }
   sh 'docker push 87545056278072821043/myweb:0.0.2'
   }
   stage('Nexus Image Push'){
   sh "docker login -u admin -p Kalainirmal@777 52.66.25.245:8083"
   sh "docker tag 87545056278072821043/myweb:0.0.2 52.66.25.245:8083/damo:1.0.0"
   sh 'docker push 52.66.25.245:8083/damo:1.0.0'
   }
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest 87545056278072821043/myweb:0.0.2' 
   }
}
}




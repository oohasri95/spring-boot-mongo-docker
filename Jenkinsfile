node{
     
    stage('SCM Checkout'){
        git credentialsId: 'GIT_CREDENTIALSS', url: 'https://github.com/manikarnam/spring-boot-mongo-docker.git',branch: 'master'
    }
   
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.6.1", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
	    sh "sudo docker build -t maniengg/spring-boot-mongo:${BUILD_ID} ."
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'DOKCER_HUB_PASSWORD', variable: 'DOKCER_HUB_PASSWORD')]) {
          sh "sudo docker login -u maniengg -p ${DOKCER_HUB_PASSWORD}"
        }
        sh "sudo docker push maniengg/spring-boot-mongo:${BUILD_ID}"
     }
     
    /** stage("Deploy To Kuberates Cluster"){
       kubernetesDeploy(
         configs: 'springBootMongo.yml', 
         kubeconfigId: 'mykubeconfig',
         enableConfigSubstitution: true
        )
     }**/
	 
      stage("Deploy To Kuberates Cluster"){
	sh "sed -i -e 's,image_to_be_deployed,'maniengg/spring-boot-mongo:${BUILD_ID}',g' springBootMongo.yml"
	      
        sh "kubectl create -f springBootMongo.yml"
	
      }
     
}



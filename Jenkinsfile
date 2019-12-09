pipeline {
   agent any
    stages{
       stage('Build'){
          steps {
                sh 'mvn -Dmaven.test.skip=true install'      
                sh 'docker rmi -f "$(docker images -aq)"'
                sh "docker build -t addapi:${env.BUILD_ID} addapi/"
                sh "docker build -t addsvc:${env.BUILD_ID} addsvc/"               
                sh 'docker login -u skumar24 -p kukku@240892'
                sh "echo sameer"
                sh "docker tag addapi:${env.BUILD_ID} skumar24/addapi:${env.BUILD_ID}"
                sh "docker tag addsvc:${env.BUILD_ID} skumar24/addsvc:${env.BUILD_ID}"                   
                sh "docker push skumar24/addapi:${env.BUILD_ID}"
                sh "docker push skumar24/addsvc:${env.BUILD_ID}"
          }
       }
     stage('DeployToProduction') {
            		steps {
                		kubernetesDeploy(
                    			kubeconfigId: 'KUBERNETES_CLUSTER_CONFIG',
                    			configs: 'webdev.yml'
                		)
            		}
        	}
   }
}

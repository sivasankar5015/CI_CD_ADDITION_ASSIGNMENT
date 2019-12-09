pipeline {
   agent any
    stages{
       stage('Build'){
          steps {
                sh 'mvn -Dmaven.test.skip=true install'      
                sh 'docker rmi -f "$(docker images -aq)"'
                sh "docker build -t addapi:${env.BUILD_ID} addapi/"
                sh "docker build -t addsvc:${env.BUILD_ID} addsvc/"               
                sh 'docker login -u siva3100 -p Siva@5015'
                sh 'pwd'
                sh "docker tag addapi:${env.BUILD_ID} siva3100/addapi:${env.BUILD_ID}"
                sh "docker tag addsvc:${env.BUILD_ID} siva3100/addsvc:${env.BUILD_ID}"                   
                sh "docker push siva3100/addapi:${env.BUILD_ID}"
                sh "docker push siva3100/addsvc:${env.BUILD_ID}"
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

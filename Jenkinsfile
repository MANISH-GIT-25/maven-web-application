pipeline {
        agent any
        stages {
	        stage("SCM") {
                     steps {
                            git 'https://github.com/MANISH-GIT-25/maven-web-application.git'
                            }
                          }

                stage("build") {
                     steps {
                             sh 'sudo mvn clean package'
                             }
                           }
                stage("build-image") {
                     steps {
                             sh 'sudo docker build -t java-repo:$BUILD_TAG .'
                             sh 'sudo docker tag java-repo1:$BUILD_TAG MANISH-GIT-25/pipeline-java1:$BUILD_TAG'
                             }
                }
                stage("dockerlogin") {
                     steps { 
		             withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'dockerhub_pass_var')]) {
			     sh 'sudo docker login -u MANISH-GIT-25 -p ${dockerhub_pass_var}'
			     sh 'sudo docker push MANISH-GIT-25/pipeline-java1:$BUILD_TAG'
			     }
		     }
		      
		}
		stage("QAT TESTING") {
		     steps {  
		              sh 'sudo docker run -dit --name webtom -p 8090:8080 MANISH-GIT-25/pipeline-java1:$BUILD_TAG'
                    } 
	       }


	        stage("test-website") {
	             steps { 
		             sh 'sudo sleep 20'
		             sh 'sudo curl --ipv4 http://localhost:8090'

                       }
	       }
}
}




















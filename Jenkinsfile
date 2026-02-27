pipeline {
    agent any

    stages {

        stage("SCM") {
            steps {
                git 'https://github.com/MANISH-GIT-25/maven-web-application.git'
            }
        }

        stage("Build") {
            steps {
                sh 'mvn clean package'
            }
        }

        stage("Build Image") {
            steps {
                sh """
                docker build -t java-repo:${BUILD_TAG} .
                docker tag java-repo:${BUILD_TAG} MANISH-GIT-25/pipeline-java1:${BUILD_TAG}
                """
            }
        }

        stage("Docker Login") {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_pass', variable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u MANISH-GIT-25 --password-stdin
                    docker push MANISH-GIT-25/pipeline-java1:${BUILD_TAG}
                    """
                }
            }
        }

        stage("QAT Testing") {
            steps {
                sh """
                docker run -dit --name webtom -p 8090:8080 \
                MANISH-GIT-25/pipeline-java1:${BUILD_TAG}
                """
            }
        }

        stage("Test Website") {
            steps {
                sh 'sleep 20'
                sh 'curl --ipv4 http://localhost:8090'
            }
        }
    }
}

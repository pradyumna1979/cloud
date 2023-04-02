pipeline {
    agent any 
    stages {
        stage('Compile and Clean') { 
            steps {

                sh "mvn clean compile"
            }
        }
        
        stage('deploy') { 
            steps {
                sh "mvn package"
            }
        }


        stage('Build Docker image'){
            steps {
                sh 'docker build -t pradyumnakhadanga1979/springboot-docker:${BUILD_NUMBER} .'
            }
        }

        stage('Docker Login'){
            
            steps {
                 withCredentials([usernamePassword(credentialsId: 'dockerCredential', passwordVariable: 'dockerPass', usernameVariable: 'dockerUser')]) {
                   sh "docker login -u ${dockerUser} -p ${dockerPass}"
               }
            }                
        }

        stage('Docker Push'){
            steps {
                sh 'docker push pradyumnakhadanga1979/springboot-docker:${BUILD_NUMBER}'
            }
        }
        
        stage('Docker deploy'){
            steps {
                sh 'docker container run -p 8081:8081 pradyumnakhadanga1979/springboot-docker:${BUILD_NUMBER}'
            }
        }

        
        stage('Archving') { 
            steps {
                 archiveArtifacts '**/target/*.jar'
            }
        }
    }
}

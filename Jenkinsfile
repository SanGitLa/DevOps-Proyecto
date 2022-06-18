pipeline {
    agent any
    tools {
        maven "M3_8_6"
         }
    stages {

        stage('Compile') {
            steps {
                dir ("Servicios/Curso-Microservicios"){
                    sh "docker build -t microservicio ."
               }  
                
            }
        }
          stage('Push Image') {
            steps {
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'docker_nexus', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    sh 'docker login 192.168.100.163:8083 -u $USERNAME -p $PASSWORD'
                    sh 'docker tag microservicio:latest 192.168.100.163:8081/repository/docker-private/:latest
                    sh 'docker push 192.168.100.163:8081/repository/docker-private/:latest'
                }
            }  
        }
    }
}

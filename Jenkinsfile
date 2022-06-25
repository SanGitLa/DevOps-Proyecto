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
                    sh 'docker tag microservicio:latest 192.168.100.163:8083/repository/docker-private/microservicio:latest'
                    sh 'docker push 192.168.100.163:8083/repository/docker-private/microservicio:latest'
                }
            }  
        }
    stage('Compile') {
            steps {
                dir ("Liquibase/"){
                    sh '/opt/liquibase/liquibase --changeLogFile="changesets/db.changelog-master.xml" update'
               }  
            }
        }
    }
}
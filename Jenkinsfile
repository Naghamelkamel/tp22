pipeline {
 agent any
 options { timestamps() }
 environment {
 IMAGE = 'elkamelnagham/monapp'
 TAG = "build-${env.BUILD_NUMBER}"
 }
  stages {
 stage('Checkout') {
 steps { checkout scm } // lit le même repo que le job
 }
 stage('Docker Build') {//creation de l'image li hiya monapp
 steps {
 bat 'docker version'
 bat "docker build -t %IMAGE%:%TAG% ."
 }
 }
 stage('Smoke Test') {// test lel etat basique 
 steps {
  //5ater 9a3din ne5dmou b windows
 bat """
 docker rm -f monapp_test 2>nul || ver > nul
 docker run -d --name monapp_test -p 8082:80 %IMAGE%:%TAG%
 ping -n 3 127.0.0.1 > nul
 curl -I http://localhost:8081 | find "200 OK"
 docker rm -f monapp_test
 """
 }
 }
 stage('Push (Docker Hub)') {
 steps {
 withCredentials([usernamePassword(credentialsId: '13',
 usernameVariable: 'USER',
passwordVariable: 'PASS')]) {
 bat """
 echo %PASS% | docker login -u %USER% --password-stdin
 docker tag %IMAGE%:%TAG% %IMAGE%:latest
 docker push %IMAGE%:%TAG%
 docker push %IMAGE%:latest
 """
 }
 }
 }
 }
 post {
 success { echo 'Build+Test+Push OK' }
 failure { echo 'Build/Tests/Push KO' }
 }
}

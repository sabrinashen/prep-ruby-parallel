pipeline {
  agent any
  environment {
    // docker path: /usr/local/bin
    // bundle path: /Users/ericyang/.rbenv/shims
    PATH = "$PATH:/usr/local/bin:/Users/ericyang/.rbenv/shims"
  }
    
  stages {
    stage('docker-compose up') {
      steps {
        //step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose.yml', option: [$class: 'StartService', scale: 5, service: 'chrome'], useCustomDockerComposeFile: true])
      	sh 'sudo docker-compose up -d'
      	sh 'sudo docker-compose scale chrome=10'
      	sh 'sudo docker ps -a'
      	sh 'bundle install'
      }
    }
      
    stage('run script') {
      steps {
      	parallel(
               "google-module":{
               	sh 'bundle exec parallel_rspec test/google/'
               },
               "baidu-module":{
               	sh 'bundle exec parallel_rspec test/baidu/'
               }
        )
      }
    }
    
     //stage('selenium grid down') {
       //steps {
         //sh 'echo "shut down selenium grid"'
   	     //sh 'sudo docker-compose down'
   	     ////step([$class: 'DockerComposeBuilder', dockerComposeFile: 'docker-compose.yml', option: [$class: 'StopAllServices'], useCustomDockerComposeFile: true])
       //}
     //}
  }
  
  post {
	always {
		sh 'echo "selenium grid down"'
		sh 'sudo docker-compose down'
		archiveArtifacts artifacts: 'output/log/**', fingerprint: true
  	}
  }
}
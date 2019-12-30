pipeline {
   agent none
   stages {
      stage('Source Code') {
         agent { label 'test' }
         steps {
            git branch: 'devel', url: 'https://github.com/hemanth22/Capstone.git'
         }
      }
      stage('Build') {
         agent { label 'test' }
         steps {
            sh "sudo docker build -t intellipaat:1.0 ."
         }
      }
      stage('Run container') {
         agent { label 'test' }
         steps {
            sh "sudo docker run -d -p 80:80 --name=intellipaat intellipaat:1.0"
         }
      }
      stage('Website test') {
         agent { label 'test' }
         steps {
            sh "hostname -I"
            sh "curl -I localhost:80"
         }
      }
      stage('clean test server') {
         agent { label 'test' }
         steps {
            sh "sudo docker rm -f \$(docker ps -q)"
            sh "sudo docker rmi -f \$(docker images -q)"
         }
      }
      stage('Deploy to prod server') {
         agent { label 'prod' }
         when { branch 'master' }
         steps {
            git 'https://github.com/hemanth22/website.git'
            sh "sudo docker build -t intellipaat:1.0 ."
            sh "sudo docker run -d -p 80:80 --name=intellipaat intellipaat:1.0"
         }
      }
   }
}

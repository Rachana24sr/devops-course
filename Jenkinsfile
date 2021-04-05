pipeline {
  environment {
    imagename = "rachanasr/devops-course"
    registryCredential = 'docker-id'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git([url: 'https://github.com/Rachana24sr/devops-course.git', branch: 'master', credentialsId: 'github-ssh-key'])
 
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')
 
          }
        }
      }
    }
    stage('Deploy image and Remove Unused  image') {
      steps{
	sh "docker stop devops"
        sh "docker run -d -p 80:80 --name devops $imagename:$BUILD_NUMBER"
        sh "docker rmi $imagename:$BUILD_NUMBER"
	sh "docker rmi $imagename:latest"
 
      }
    }
  }
}

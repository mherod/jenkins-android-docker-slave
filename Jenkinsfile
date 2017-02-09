node('linuxee-docker') {
    
   def containerName="linuxee-docker-android"
   def imageName="jenkins-android-docker-slave"
   def image
   def options="--restart=always --name $containerName -v /var/run/docker.sock:/var/run/docker.sock"
   withCredentials([string(credentialsId: 'jenkins-slave-secret', variable: 'key'), string(credentialsId: 'jenkins-slave-register-url', variable: 'url'), string(credentialsId: 'jenkins-slave-tunnel', variable: 'tunnel')]) {
     def args="-headless -tunnel $tunnel -url $url $key $containerName"
      
     stage('Checkout') {
       checkout scm     
     }
     stage('Stop container') {
       sh "docker stop $containerName || echo $containerName is not running"
     }
   
     stage('Remove container') {
      sh "docker rm $containerName || echo $containerName has been already removed"
     }
   
     stage('Build image') {
       image = docker.build("$imageName")
     }
   
     stage('Run container') {
      image.run("$options","$args")
     }
   }
}

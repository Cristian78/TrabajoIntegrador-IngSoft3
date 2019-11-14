node {
   def mvnHome
   def image
   stage('Preparation') {
      git 'https://github.com/Cristian78/TrabajoIntegrador-IngSoft3.git'
      mvnHome = tool 'M3'
   }
   stage('Build') {
      withEnv(["MVN_HOME=$mvnHome"]) {
        sh 'cd server && "$MVN_HOME/bin/mvn" -Dmaven.test.failure.ignore clean package'
		image = docker.build("cristian78/trabajointegradoringsoft3")
      }
   }
   stage('Sonar Cloud') {
	   withEnv(["MVN_HOME=$mvnHome"]) {
	   sh 'cd server && "$MVN_HOME/bin/mvn" verify sonar:sonar -Dsonar.projectKey=Cristian78_TrabajoIntegrador-IngSoft3 -Dsonar.organization=cristian78 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=07df0dcc1142ecfe155de768407f1a3f5e0facd0 -Dmaven.test.failure.ignore=true'
	}
   }
   stage('Push Image') {
       docker.withRegistry('', 'dockerhub') {
	   image.push()
	 }
   }
   stage('Results') {
      archiveArtifacts 'server/target/*.jar'
      junit '**/target/surefire-reports/TEST-*.xml'
   }
   stage('Deploy') {
     withCredentials([usernamePassword(credentialsId: 'herokuCredentials', passwordVariable: 'password', usernameVariable: 'user')]) {
	   //sh 'docker login --username=_ --password=${password} registry.heroku.com'
	   //sh 'docker tag  registry.heroku.com/rocky-brushlands-25964/web'
	   //sh 'docker push registry.heroku.com/rocky-brushlands-25964/web'
	   //sh 'heroku container:release web --app=rocky-brushlands-25964'
    }
  }
}

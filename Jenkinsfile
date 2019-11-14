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
	    sh 'cd server && "$MVN_HOME/bin/mvn" verify sonar:sonar -Dsonar.projectKey=Cristian78_TrabajoIntegrador-IngSoft3 -Dsonar.organization=cristian78 -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=c69a2068552a560120f4d5a31b244b31dcd2d567 -Dmaven.test.failure.ignore=true'
	}
   }
   stage('Push Image') {
     docker.withRegistry('', 'dockerhub') {
	   image.push()
	 }
   }
   stage('Results') {
      //archiveArtifacts 'server/target/*.jar'
      //junit '**/target/surefire-reports/TEST-*.xml'
   }
   stage('Deploy') {
     withCredentials([usernamePassword(credentialsId: 'herokuCredentials', passwordVariable: 'password', usernameVariable: 'user')]) {
		sh 'heroku login'
		sh 'echo ${user}'
		sh 'echo ${password}'
		sh 'heroku container:push web --app=rocky-brushlands-25964'
		//sh 'echo ${user}'
		//sh 'echo ${password}'
		sh 'heroku container:release web --app=rocky-brushlands-25964'
		//sh 'echo ${user}'
		//sh 'echo ${password}'
        }
      //def heroku_auth = 29c64bd7-9da5-4f06-8cff-51cd7f6ae6b4
      //sh 'docker login --username=_ --password=29c64bd7-9da5-4f06-8cff-51cd7f6ae6b4 registry.heroku.com'
      //sh 'heroku container:push web --app=rocky-brushlands-25964'
	  //sh 'docker push registry.heroku.com/rocky-brushlands-25964'
	  //sh 'heroku container:release web --app=rocky-brushlands-25964'
   }
}

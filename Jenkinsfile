node ('master') {
ansiColor('xterm') {
  stage('Clone repository') {
        checkout scm
    }
    
   // stage('Check-Git-Secrets') {
   //  	sh "trufflehog --exclude_paths exclude-patterns.txt --regex https://github.com/imkirannn/vuls-java-app.git"
//	}


 stage ('Source Composition Analysis') {
	sh '''
	wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh"
	chmod +x owasp-dependency-check.sh && bash owasp-dependency-check.sh
	cp -rv /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.html ~/OWASP
	'''
	
 }
 stage ('SAST Sonar') {
	withMaven(maven: 'Maven_3.6.3'){
	sh '''
	mvn clean install
	mvn package  org.jacoco:jacoco-maven-plugin:prepare-agent sonar:sonar \
	-Dsonar.host.url=http://ci-jenkins.cloudhands.online/ \
	-Dsonar.login=c4340e520f9802c0137132d5e48905f140dba8a7 	
	'''
	}
    }
} 
	
}

node ('master') {
  stage('Check-Git-Secrets') {
	sh '''
	rm trufflehog || true
        docker run gesellix/trufflehog --json https://github.com/imkirannn/webapp.git > trufflehog
        cat trufflehog
	'''
	}
 stage('Clone repository') {
        checkout scm
    }
 stage ('Source Composition Analysis') {
	sh '''
	rm owasp* || true
	wget "https://raw.githubusercontent.com/cehkunal/webapp/master/owasp-dependency-check.sh"
	chmod +x owasp-dependency-check.sh && bash owasp-dependency-check.sh
	cp -rv /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml ~/OWASP
	'''
	}
 stage ('SAST Sonar') {
	sh '''
	mvn  org.jacoco:jacoco-maven-plugin:prepare-agent sonar:sonar \
	-Dsonar.host.url=http://ci-jenkins.cloudhands.online/ \
	-Dsonar.login=c4340e520f9802c0137132d5e48905f140dba8a7	
	-Dsonar.projectKey=Sast-for-JavaApp	
	'''
	}
} 
	

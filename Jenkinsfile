node {
    def mavenHome = tool name: "maven3.9.10"

    stage('Cloning') {
        git branch: 'dev', url: 'https://github.com/kirankumar-3/maven-webapplication-project-kkfunda.git'
    }

    stage('compile') {
        sh "${mavenHome}/bin/mvn compile"
    }
    stage('build')
    {
        sh "${mavenHome}/bin/mvn clean package"
    }
    stage('sonar')
    {
        sh "${mavenHome}/bin/mvn clean package sonar:sonar "
    }
    stage('deploy')
    {
        sh "${mavenHome}/bin/mvn clean deploy "
    }
 stage('Deploy to Tomcat') {
    withCredentials([usernamePassword(
        credentialsId: 'tomcat-creds',
        usernameVariable: 'TOMCAT_USER',
        passwordVariable: 'TOMCAT_PASS'
    )]) {

        sh '''
        curl -v -u $TOMCAT_USER:$TOMCAT_PASS \
        -T target/*.war \
        "http://54.87.149.65:8080/manager/text/deploy?path=/myapp&update=true"
        '''
    }
}
    /*  stage('Deploy to Tomcat') 
    {
      
      sh """

      curl -u kiran:12345678 \
--upload-file /var/lib/jenkins/workspace/airtel-pipeline/target/maven-web-application.war \
"http://54.87.149.65:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
    }*/
}/*pipeline
{
	agent any
	tools
	{
	  maven "maven-3.9.14"
	}
	stages
	{
	 stage('checkout')
	 {
	    steps
	    {
	   git branch: 'dev', url: 'https://github.com/kkdevopsb8/maven-webapplication-project-kkfunda.git'
	   }
	 }
	 stage('Build')
	 {
	   steps
	   {
	      sh "mvn clean package"
	   }
	 }
	 stage('SQ REPORT')
	 {
	   steps
	   {
	   sh "mvn sonar:sonar"
	   }
	 }
	 stage('Upload to nexus')
	 {
	     steps
	     {
	    sh "mvn deploy"
	     }
	 }
	 stage('Deploy to tomcat')
	 {
	   steps
	   {
	      sh '''
            curl -u kk:password \
            --upload-file /var/lib/jenkins/workspace/Declarative-PL-Dev/target/maven-web-application.war \
            "http://13.232.26.179:8080/manager/text/deploy?path=/maven-web-application&update=true"
            '''
	   }
	 }

	} //stages  ending

} //pipeline ending
*/

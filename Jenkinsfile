


//declarative pipeline
pipeline {
    agent any

    tools {
        maven "maven3.9.10"
    }

    environment {
        SLACK_CHANNEL = '#devops-team'   // 🔁 CHANGE THIS to your channel
    }

    stages {

        stage('Notify Start') {
            steps {
                slackSend (
                    channel: "${SLACK_CHANNEL}",
                    color: '#FFFF00',
                    message: "STARTED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
                )
            }
        }

        stage ('clone') {
            steps {
                git branch: 'dev', url: 'https://github.com/kirankumar-3/maven-webapplication-project-kkfunda.git'
            }
        }

        stage ('build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage ('report') {
            steps {
                sh "mvn sonar:sonar"
            }
        }

        stage ('deploy to nexus') {
            steps {
                sh "mvn deploy"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'tomcat-creds',  // 🔁 CHANGE THIS to your tomact id
                    usernameVariable: 'TOMCAT_USER',
                    passwordVariable: 'TOMCAT_PASS'
                )]) {

                    sh '''
                    curl -v -u $TOMCAT_USER:$TOMCAT_PASS \
                    -T target/*.war \
                    "http://34.201.120.253:8080/manager/text/deploy?path=/myapp&update=true" // 🔁 CHANGE THIS to your tomcat IP
                    '''
                }
            }
        }
    }

    post {

        success {
            slackSend (
                channel: "${SLACK_CHANNEL}",
                color: '#00FF00',
                message: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            )
        }

        failure {
            slackSend (
                channel: "${SLACK_CHANNEL}",
                color: '#FF0000',
                message: "FAILURE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            )
        }

        unstable {
            slackSend (
                channel: "${SLACK_CHANNEL}",
                color: '#FFA500',
                message: "UNSTABLE: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})"
            )
        }
    }
}


//scripted way pipeline
/*node {
	def mavenHome = tool name: "maven3.9.10"
	echo "git branch Name: ${env.BRANCH_NAME}"
echo "build number: ${env.BUILD_NUMBER}"
try{

    
				
    stage('Cloning') {
        git branch: 'dev', url: 'https://github.com/kirankumar-3/maven-webapplication-project-kkfunda.git'
    }

    stage('compile') {
		notifyBuild('STARTED')
        sh "${mavenHome}/bin/mvn compile"
    }

    stage('build') {
        sh "${mavenHome}/bin/mvn clean package"
    }

    stage('sonar') {
        sh "${mavenHome}/bin/mvn sonar:sonar"
    }

    stage('deploy') {
        sh "${mavenHome}/bin/mvn deploy"
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
            "http://34.201.120.253:8080/manager/text/deploy?path=/myapp&update=true"
            '''
        }
    }
	
}//try end
	 catch (e) {
   
       currentBuild.result = "FAILED"

  } finally {
    // Success or failure, always send notifications
    notifyBuild(currentBuild.result)       //function calling
  }
}  //node end
def notifyBuild(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#278EF5'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }

  // Send notifications
  slackSend (color: colorCode, message: summary, channel: '#devops-team')
  
}

/*  stage('Deploy to Tomcat') 
    {
      
      sh """

      curl -u kiran:12345678 \
--upload-file /var/lib/jenkins/workspace/airtel-pipeline/target/maven-web-application.war \
"http://54.87.149.65:8080/manager/text/deploy?path=/maven-web-application&update=true"
          
        """
    }*/

/*pipeline
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
*/

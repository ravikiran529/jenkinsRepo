def remote = [:]
  remote.name = 'QA-Server'
  remote.host = '192.168.35.15'
  remote.user = 'root'
  remote.password = 'vagrant'
  remote.allowAnyHosts = true
def mvnHome
pipeline {
   agent {
       label "slave1"
   }       
   stages {
       stage('Code from GitHub') {
	        steps {
			    git 'https://github.com/ravikiran529/Maven-Java-Project.git'
				stash "Source"
				script{
			        mvnHome = tool 'maven3.6'
			    }
			}
       }		
	   stage('Build') {
	        steps {
			    sh "'${mvnHome}/bin/mvn' clean package"
			}		
			post {
			    always {
				      archiveArtifacts artifacts: '**/*.war', onlyIfSuccessful: true
				  }
			}
	   }	
       stage('Remote SSH') {
             steps {
             // writeFile file: 'abc.sh', text: 'ls -lrt'
			    unstash "Source"
                sshPut remote: remote, from: 'target/java-maven-1.0-SNAPSHOT.war', into: '/root/tomcat-server/webapps'
             }    
       }
	}
}	

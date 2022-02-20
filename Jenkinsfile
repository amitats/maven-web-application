node {

properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '5')), 
pipelineTriggers([pollSCM('* * * * *')])])
def mavenHome = tool name: "maven3.8.3"

stage ('CheckOutCode') { 

git branch: 'development', credentialsId: 'c37adda7-9bf5-4ad1-b604-febd155f97d9', url: 'https://github.com/amitats/maven-web-application.git'

}


stage ('CodeBuild') { 
sh "${mavenHome}/bin/mvn clean package"
}



stage ('Executesonarcube') { 
sh "${mavenHome}/bin/mvn sonar:sonar"
}


stage ('Uploadartifactsintonexus') { 
sh "${mavenHome}/bin/mvn deploy"
}


stage ('deploontomcat') { 
sshagent(['37f290bd-44db-4515-8c8c-1aa5001aaf10']) {
sh "scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/pipeline-scriptedway/target/maven-web-application.war ec2-user@3.68.231.36:/opt/tomcat9/webapps/"
}
}


}

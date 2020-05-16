currentBuild.displayName = "gameoflife - #"+currentBuild.number
pipeline{
         agent {

        label "master"
	 }
    stages{
         stage('Checkout-SCM'){
		     steps{
			  checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
				    doGenerateSubmoduleConfigurations: false, 
				    extensions: [], 
				    submoduleCfg: [],
				    userRemoteConfigs: [[credentialsId: '2a239424-0e74-41e9-8034-488a00f90311',
			            url: 'https://github.com/HariReddy910/game-of-life.git']]])
			    }
		         }
         stage('Build-stage'){ 
		      steps{ 
			   sh label: '', script: 'mvn package' 
		           } 
	                 }
         stage("archiveArtifacts"){
                      steps{
                           archiveArtifacts '**/*.war'
                         }
                      }
           stage("deploy-to-appserver"){
                      steps{
	   sh label: '', script: 'scp /var/jenkins_home/workspace/Game-of-life@2/gameoflife-web/target/gameoflife.war ubuntu@172.31.26.148:/var/lib/tomcat9/webapps/gameoflife.war'
		      }
	   }
             stage('upload') {
           steps {
              script { 
                 def server = Artifactory.server 'Artifactory-1'
                 def uploadSpec = """{ 
                   "files": [{
                       "pattern": "**/*.war",
                       "target": "jfrog-release-artifactory"
                       
                    }]
                 }"""
                 
                 server.upload(uploadSpec) 
            }
       }
     }
  }

    }

}
		      


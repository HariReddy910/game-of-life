currentBuild.displayName = "gameoflife - #"+currentBuild.number
pipeline{
     agent any
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
	   
	    
        }
}

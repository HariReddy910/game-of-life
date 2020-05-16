currentBuild.displayName = "gameoflife - #"+currentBuild.number
pipeline{
         agent {

        label "master"
               }
	    environment {

        // This can be nexus3 or nexus2

        NEXUS_VERSION = "nexus3"

        // This can be http or https

        NEXUS_PROTOCOL = "http"

        // Where your Nexus is running

        NEXUS_URL = "3.136.87.11:8081"

        // Repository where we will upload the artifact

        NEXUS_REPOSITORY = "nexus-repo"

        // Jenkins credential id to authenticate to Nexus OSS

        NEXUS_CREDENTIAL_ID = "2a239424-0e74-41e9-8034-488a00f90311"

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
                stage("publish to nexus") {

            steps {

                script {

                    // Read POM xml file using 'readMavenPom' step , this step 'readMavenPom' is included in: https://plugins.jenkins.io/pipeline-utility-steps

                    pom = readMavenPom file: "pom.xml";

                    // Find built artifact under target folder

                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");

                    // Print some info from the artifact found

                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"

                    // Extract the path from the File found

                    artifactPath = filesByGlob[0].path;

                    // Assign to a boolean response verifying If the artifact name exists

                    artifactExists = fileExists artifactPath;

                    if(artifactExists) {

                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";

                        nexusArtifactUploader(

                            nexusVersion: NEXUS_VERSION,

                            protocol: NEXUS_PROTOCOL,

                            nexusUrl: NEXUS_URL,

                            groupId: pom.groupId,

                            version: pom.version,

                            repository: NEXUS_REPOSITORY,

                            credentialsId: NEXUS_CREDENTIAL_ID,

                            artifacts: [

                                // Artifact generated such as .jar, .ear and .war files.

                                [artifactId: pom.artifactId,

                                classifier: '',

                                file: artifactPath,

                                type: pom.packaging],

                                // Lets upload the pom.xml file for additional information for Transitive dependencies

                                [artifactId: pom.artifactId,

                                classifier: '',

                                file: "pom.xml",

                                type: "pom"]

                            ]
                                                    );

                    } else {

                        error "*** File: ${artifactPath}, could not be found";

                    }

                }

            }

        }

    }

}
		      


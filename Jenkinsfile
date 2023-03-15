pipeline{
        agent none
	//agent {label 'UBUNTU'}
                
	triggers{
            cron('* * * * *')
        }
	parameters {
	choice(name: 'MAVEN_GOAL', choices: ['clean', 'install', 'package'], defaultValue: 'clean', description: 'Pick something')	
	}
	stages{
		stage('git'){
			steps{
					git url: 'https://github.com/dwarasree2222/spring-petclinic.git',
						branch: 'main'
				}
		}
		stage('shell'){
			steps{
					sh 'export PATH="/usr/lib/jvm/java-1.17.0-openjdk-amd64/bin:$PATH"'
					sh "mvn ${params.MAVEN_GOAL}"
					  //sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'
			}   

		}
		stage('archiving'){
			steps{
					archiveArtifacts '**/target/spring*.jar'
			}
		}
		
		/*stage('build && SonarQube analysis') {
                         steps {
                                        withSonarQubeEnv('SonarCloud') {
                                        // Optionally use a Maven environment you've configured already
                                        //withMaven(maven:'Maven 3.5') {
					
                                        sh 'mvn clean package sonar:sonar -Dsonar.organization=spring-petclinic'
                        }
                }
        
       	        stage("Quality Gate"){
                        steps {
                                        timeout(time: 1, unit: 'HOURS'){
                                        waitForQualityGate abortPipeline: true
			}
                }*/
        

    }
}

pipeline{
        //agent any
	agent {label 'UBUNTU'}
                
	triggers{
            cron('H 23 * * 1-5')
        }
	parameters {
	choice(name: 'MAVEN_GOAL', choices: ['clean', 'install', 'package'], description: 'Pick something')	
	}
	stages{
		stage('git'){
			steps{
					git url: 'https://github.com/dwarasree2222/spring-petclinic.git',
						branch: 'sprint-release-branch'
				}
		}
		stage('shell'){
			steps{
					sh 'export PATH="/usr/lib/jvm/java-1.17.0-openjdk-amd64/bin:$PATH"'
					sh "mvn ${params.MAVEN_GOAL}"
					  //sh 'export PATH="/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin:$PATH" && mvn package'
                                        sh java -jar spring-petclinic-3.0.0-SNAPSHOT.jar
			}   

		}
		stage('archiving and stash'){
			steps{
					archiveArtifacts artifacts: '**/target/spring*.jar'
                                        stash name: 'spc',
				              includes: '**/target/spring*.jar'
			}
		}
                stage('unstash'){
			steps{
					unstash name:'spc'
			}
		
		}  
                stage('build && SonarQube analysis') {
                         steps {
                                        withSonarQubeEnv('SonarCloud') {
                                        sh 'mvn install package sonar:sonar -Dsonar.organization=spring-petclinic'
                                        }
                         }
                }
        
       	        stage("Quality Gate"){
                        steps {
                                        timeout(time: 1, unit: 'HOURS'){
                                        waitForQualityGate abortPipeline: true
			                }
                        }
                }

    }
}

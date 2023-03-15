pipeline{
        agent any
	//agent {label 'UBUNTU'}
                
	//triggers{
        //    cron('* * * * *')
       // }
	parameters {
	choice(name: 'MAVEN_GOAL', choices: ['clean', 'install', 'package'], description: 'Pick something')	
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

    }
}

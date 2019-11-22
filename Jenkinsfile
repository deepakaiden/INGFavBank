pipeline {
	agent any
		stages {
			/**INGFAV_BANK-Backend Pipeline Job Build and Test stages **/
			stage('SCM Checkout') {
				steps {
					git url:'https://github.com/amar5182/INGFavBank.git'
						}
								}
			stage('Build') {
				steps {
                        sh"mvn clean package -Dmaven.test.skip=true"
							}
					}
			stage('Build Analysis') { 
          			 steps {
					 withSonarQubeEnv('gate1') { 
                			  sh 'mvn clean verify -Dmaven.test.skip=true sonar:sonar' 
               						}
						   } 
       				  } 
			stage("Quality Gate") {
           			 steps {
             				 timeout(time: 5, unit: 'MINUTES') {
               					 waitForQualityGate abortPipeline: true
         					     }
          				  }
         			 }
			stage('Deploy') {
				steps {
                        sh"mvn clean deploy -Dmaven.test.skip=true"
							}
					}
			stage('Release') {
				steps {
                        sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
							}
					}
				}
		}

pipeline{
    agent any
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages{
        stage("Cleanup Workspace"){
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM"){
            steps {
               git branch: 'master', credentialsId: 'github', url: 'https://github.com/siddharthpatel1993/java_test'
            }
        }

        stage("Build Application"){
            steps {
               sh "mvn clean package"
            }
        }
		
        stage("Test Application"){
            steps {
               sh "mvn test"
            }
        }
		
        stage("Sonarqube Analysis") {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'jenkins_sonarqube_token') {
                        sh "mvn sonar:sonar"
                    }
                }
            }

        }
		
        stage("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins_sonarqube_token'
                }
            }

        }		
    }
}

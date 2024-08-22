pipeline{
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('checkout the code'){
            steps{
                git url:'https://github.com/gitchegde/jenkins-maven', branch: 'master'
            }
        }
        stage('build the code'){
            steps{
                sh 'mvn clean package'
            }
        }
	stage("sonar quality check"){
            steps{
                script{
                    withSonarQubeEnv(installationName: 'sonar', credentialsId: 'sqube') {
                            sh 'mvn sonar:sonar '
                    }

                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }

                }  
            }
        }

}
}

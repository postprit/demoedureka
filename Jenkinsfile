pipeline { 
    agent any 
	//clone the git
    stages {
        stage('Git Clone') {
            steps {
                echo "checking out the repo from git"
               // git clone 'https://github.com/postprit/demoedureka.git'
            
            }
        }
		//Build & deploy
        stage('Docker build and deploy container'){
            steps { 
                script {
                    build job: 'docker_deployment' , parameters: [
                        [$class: 'StringParameterValue', name: 'PARAM_PARENT_BUILD', value: "${BUILD_NUMBER}"]
                        ]
                }
            }
        }
		//testcase
        stage('Triggering Selenium Test Case'){
            steps {
                script {
                   try {
                    build job: 'selenium_test' , parameters: [
                        [$class: 'StringParameterValue', name: 'PARAM_PARENT_BUILD', value: "${BUILD_NUMBER}"]
                        ]
                       }catch(err){
                          build job: 'delete_docker_container' , parameters: [
                        [$class: 'StringParameterValue', name: 'PARAM_PARENT_BUILD', value: "${BUILD_NUMBER}"]
                        ]
                          }
                   }
            }
        }
        

     //   stage('publish html report') {
       //     steps{
         //       echo "publishing the html report"
           //     publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
//            }
  //      }
        stage('clean up') {
            steps {
                echo "cleaning up the workspace"
                cleanWs()
            }
        }
        
    }
}

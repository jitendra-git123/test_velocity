node() {
  
  def check
  stage('prepare') {
      check=checkout scm
      echo "${check}"
        echo ">>>>>>>${check.GIT_COMMIT}"
       }

  stage('build') {
      mtaBuild script: this
      step([$class: 'UploadBuild',tenantId: "5ade13625558f2c6688d15ce",revision: "${check.GIT_COMMIT}",appName: "Sapphire 2019-SAP-Jenkins-Demo",requestor: "admin",id: "${BUILD_NUMBER}"])
  }
  
  stage('Dev Deployment') {
  	build job: 'Sapphire-2019-SAP-Demo/dev_deploy', wait: false, parameters: [string(name: 'previousBuildNumber', value: BUILD_NUMBER), string(name: 'previousBuildUrl', value: BUILD_URL)]
  } 
}

node() {
  
  def GIT_COMMIT
  def majorVersion="CR-${BUILD_NUMBER}"
  def check

  stage('prepare') {
      check=checkout scm
      GIT_COMMIT = check.GIT_COMMIT
	    echo "-===>>>>>>>>>>>>>>>>>${GIT_COMMIT}"     
  }

  stage('build') {
      echo "-===>>>>>>>>>>>>>>>>>In Build stage<<<<<<<<<<<<<<<<<===-"
  }

  stage ('UCV Upload Build') {
      step([$class: 'UploadBuild',
      tenantId: "5ade13625558f2c6688d15ce",
      revision: "${GIT_COMMIT}",
      appName: "Sapphire-Jenkins",
      requestor: "admin",
      id: "${majorVersion}"
      ])
  }
  
  stage('Kick off Dev Deployment') {
  	build job: 'Velocity/sapphire_dev_deploy', wait: true, parameters: [string(name: 'previousBuildNumber', value: BUILD_NUMBER), string(name: 'previousBuildUrl', value: BUILD_URL)]
  }

}

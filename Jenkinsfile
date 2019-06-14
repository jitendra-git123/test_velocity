node() {
  
  def check
  stage('prepare') {
      check=checkout scm
      echo "${check}"
        echo ">>>>>>>${check.GIT_COMMIT}"
       }

   stage('build') {
      step([$class: 'UploadBuild',tenantId: "5ade13625558f2c6688d15ce",revision: "${check.GIT_COMMIT}",appName: "Sapphire 2019-Jenkins-Demo",requestor: "admin",id: "${BUILD_NUMBER}"])
  }
  
  stage('Dev Deployment') {
  	build job: 'Velocity/dev_deploy', wait: false, parameters: [string(name: 'previousBuildNumber', value: BUILD_NUMBER), string(name: 'previousBuildUrl', value: BUILD_URL)]
  }

stage ("build") {
                step([$class: 'UCDeployPublisher', 
                                                component: [
                                                                componentName: 'cole_demo',
                                                                componentTag: '',
                                                                delivery: [
                                                                                $class: 'Push',
                                                                                baseDir: "/var/lib/jenkins/workspace/Sapphire-2019-SAP-Demo/build-ucd",
                                                                                fileExcludePatterns: '',
                                                                                fileIncludePatterns: '**/*',
                                                                                pushDescription: '',
                                                                                pushIncremental: false,
                                                                                pushVersion: "${majorversion}"]
                                                ]])
                                                
                                newComponentVersionId = "${cole_demo_VersionId}"
                                echo "Component Version ID =>>>>>>>${newComponentVersionId}"
        step([$class: 'UploadBuild',
           tenantId: "5ade13625558f2c6688d15ce",
           revision: "${check.GIT_COMMIT}",
           appName: "cole_demo",
           requestor: "admin",
           id: "${newComponentVersionId}"
        ])
   }


stage('Publish to UrbanCode Deploy') {        
                sleep 15
                                step([$class: 'UCDeployPublisher', 
                                                deploy: [
                                                                createSnapshot: [
                                                                                deployWithSnapshot: true,
                                                                                snapshotName: "${majorversion}"],
                                                                                deployApp: 'cole_demo',
                                                                                deployDesc: 'Requested from Jenkins',
                                                                                deployEnv: 'DEV',
                                                                                deployOnlyChanged: false,
                                                                                deployProc: 'deploy',
                                                                                deployReqProps: '',
                                                                                deployVersions: "cole_demo:${majorversion}"]])
}
}

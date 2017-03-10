Skip to content
This repository
Search
Pull requests
Issues
Gist
 @rawlingsj
 Sign out
 Unwatch 1
  Star 0
 Fork 10 rawlingsj/fabric8-jenkinsfile-library
forked from fabric8io/fabric8-jenkinsfile-library
 Code  Pull requests 0  Projects 0  Wiki  Pulse  Graphs  Settings
Branch: master Find file Copy pathfabric8-jenkinsfile-library/maven/CanaryReleaseAndStage/Jenkinsfile
c66a104  16 hours ago
@rawlingsj rawlingsj fix pipelines so the run when added as jobs or by the branch plugin
2 contributors @jstrachan @rawlingsj
SourcegraphRawBlameHistory     
62 lines (46 sloc)  1.3 KB
#!/usr/bin/groovy
@Library('github.com/fabric8io/fabric8-pipeline-library@master')

def failIfNoTests = ""
try {
  failIfNoTests = ITEST_FAIL_IF_NO_TEST
} catch (Throwable e) {
  failIfNoTests = "false"
}

def localItestPattern = ""
try {
  localItestPattern = ITEST_PATTERN
} catch (Throwable e) {
  localItestPattern = "*KT"
}


def versionPrefix = ""
try {
  versionPrefix = VERSION_PREFIX
} catch (Throwable e) {
  versionPrefix = "1.0"
}

def canaryVersion = "${versionPrefix}.${env.BUILD_NUMBER}"
def utils = new io.fabric8.Utils()
def label = "buildpod.${env.JOB_NAME}.${env.BUILD_NUMBER}".replace('-', '_').replace('/', '_')

mavenNode {
  git 'https://github.com/rawlingsj/spring-boot-webmvc.git'
  if (utils.isCI()){
    
      mavenCI{}
    
  } else if (utils.isCD()){
    
    def envStage = utils.environmentNamespace('staging')

    echo 'NOTE: running pipelines for the first time will take longer as build and base docker images are pulled onto the node'
    container(name: 'maven') {

      stage 'Build Release'
      mavenCanaryRelease {
        version = canaryVersion
      }

      stage 'Rollout Staging'
      kubernetesApply(environment: envStage)

    }
  }
}




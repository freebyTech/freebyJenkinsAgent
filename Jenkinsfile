@Library('freebyTech')_

import com.freebyTech.BuildInfo

String versionPrefix = '1.0'
String repository = 'freebytech-pub'    
String imageName = 'jenkins-agent'
String dockerBuildArguments = ''
BuildInfo buildInfo

node 
{
  buildInfo = build(this, versionPrefix, repository, imageName, dockerBuildArguments, true)
}
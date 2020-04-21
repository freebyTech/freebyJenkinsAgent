def label = "worker-${UUID.randomUUID().toString()}"
// Change this when major / minor functionality changes.
def version_prefix = '1.0'
def version=''
def tag=''
def registry = ''
def repository = 'freebytech-pub'    
def image = 'jenkins-agent'
def docker_build_arguments=''


def date = new Date()
version = "${version_prefix}.${env.BUILD_NUMBER}.${date.format('MMdd')}"
      
// Standard Docker Registry or custom docker registry?
if('index.docker.io'.equalsIgnoreCase(env.REGISTRY_URL)) 
{
  echo 'Publishing to standard docker registry.'
  tag = "${repository}/${image}:${version}"
  regsitry = ''
}
else 
{
  echo "Publishing to registry ${env.REGISTRY_URL}"
  tag = "${env.REGISTRY_URL}/${repository}/${image}:${version}"
  registry = "https://${env.REGISTRY_URL}"
}  
     
podTemplate( label: label,
  containers: 
  [
    containerTemplate(name: 'docker', image: 'docker:18.06', ttyEnabled: true, command: 'cat')
  ], 
  volumes: 
  [
    hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
  ])
{
  node(label) 
  {
    stage('Setup Build Settings') 
    {
      echo '--------------------------------------------------'
      echo " Building version ${version}"
      echo " for branch ${env.BRANCH_NAME}"
      echo '--------------------------------------------------'          
      currentBuild.displayName = "# " + version
    }
    
    stage('Build Image and Publish') 
    {
      container('docker') 
      {
        checkout scm

        // Use guid of known user for registry security
        docker.withRegistry(registry, "f3cf82a8-abd5-47ae-9f2e-d9c15dde97a0") 
        {
          def app
          if(docker_build_arguments=='') 
          {
            app = docker.build(tag, "./docker")
          }
          else 
          {
            app = docker.build(tag,"--build-arg ${docker_build_arguments} ./docker")
          }
          app.push()
          if("develop".equalsIgnoreCase(env.BRANCH_NAME)) 
          {
            app.push('latest')
          }          
        }
      }
    }
  }
}
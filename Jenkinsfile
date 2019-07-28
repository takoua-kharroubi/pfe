node{
  def Namespace = "default"
  def ImageName = "takoua113/cicd"
  def Creds	= "takoua_docker_hub"
  try{
    stage('Checkout'){
      git(url: "https://github.com/takoua-kharroubi/pfe.git", branch: "${ghprbSourceBranch}")
      sh "git rev-parse --short HEAD > .git/commit-id"
      imageTag = readFile('.git/commit-id').trim()
    } 
    stage('RUN Unit Tests'){
      sh "npm install"
      sh "npm test"
    }
    stage('Docker Build') {
      sh "docker build -t ${ImageName}:${imageTag} ."
    }
    stage('Scan docker image') {
      aquaMicroscanner imageName: "${ImageName}:${imageTag}", notCompliesCmd: 'exit 1', onDisallowed: 'fail'
    }
    stage('Docker Build, Push'){
      withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {        
        sh "docker push ${ImageName}"
      }
    }    
   
  } catch (err) {
    currentBuild.result = 'FAILURE'
  }
}

node{

  try{
    stage('Checkout'){
     git(url: "https://github.com/takoua-kharroubi/pfe.git", branch: "${ghprbSourceBranch}")
    } 
    stage('RUN Unit Tests'){
      sh "npm install"
      sh "npm test"
    }
    stage('Docker Build') {
       sh "docker login -u takoua113 -p fasionhouse1993"
          sh "docker build -t takoua113/boilerplate-node-api:pr-${ghprbPullId} ."

    }
    
    stage('Docker Build, Push'){
       sh "docker push takoua113/boilerplate-node-api:pr-${ghprbPullId}"
      
    }    
    
}
}

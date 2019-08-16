def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
pipeline {
    agent any
    stages{
        stage('repoUrl'){
            steps{
            echo makeSureECRExists(ecrRepoName, region)
            }
        }
    }
}
def makeSureECRExists(ecrRepoName, region){
  try{
    repoUrl = sh (returnStdout: true, script: 'aws ecr create-repository --repository-name srinivas --region us-west-2 --output text | awk \'{print \$NF}\'')
    echo repoUrl
    return repoUrl
  }catch(error){
    echo "INFO repository already exists"
    repoUrl = sh (returnStdout: true, script: 'aws ecr describe-repositories --repository-name srinivas --region us-west-2 --output text | awk \'{print \$NF}\'')
    echo repoUrl
    return repoUrl
  }
}

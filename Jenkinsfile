def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
pipeline {
    stages{
        stage{
            steps{
            echo makeSureECRExists(ecrRepoName, region)
            }
        }
    }
}
def makeSureECRExists(ecrRepoName, region){
  try{
    def repoUrl = sh script:"aws ecr create-repository --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'",  returnStdout: true
    echo repoUrl
    return repoUrl
  }catch(error){
    echo "INFO repository already exists"
    def repoUrl = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
    echo repoUrl
    return repoUrl
  }
}

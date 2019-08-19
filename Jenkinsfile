def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
//def repo_url = ""
pipeline {
  agent any
  environment {
       repo_url = makeSureECRExists(ecrRepoName, region)
    }
    stages{
      stage('repoUrl'){
        steps{
          script{
            //checkout scm
            def commitHash = checkout(scm).GIT_COMMIT
            echo "${commitHash}"
            }
          }
        }

      stage('Create & Push DockerImage') {
        
        steps {
          script
            {      
            sh "\$(aws ecr get-login --no-include-email --region ${region}) --password-stdin"
            sh "docker build -t ${repo_url} ."
            sh "docker tag 553752123941.dkr.ecr.us-west-2.amazonaws.com/srinivas:latest 553752123941.dkr.ecr.us-west-2.amazonaws.com/srinivas:0.1" 
            sh "docker push 553752123941.dkr.ecr.us-west-2.amazonaws.com/srinivas:0.1"
          }
        }
      }
    }
  }

def makeSureECRExists(ecrRepoName, region){
  try{
    def repoUrl = sh script:"aws ecr create-repository --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
    echo "Hari------${repoUrl}"
    return repoUrl
  }catch(Exception ex){
    echo "INFO repository already exists"
  }finally{
  def repoUrl = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
  return repoUrl
  }
}

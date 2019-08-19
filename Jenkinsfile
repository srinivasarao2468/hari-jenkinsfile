def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
def repo_url = ""
pipeline {
  agent any
  environment {
      repo_url = makeSureECRExists(ecrRepoName, region)
      Image_Vesrion = "${env.repo_url}:${version}"
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
              echo 1
              docker.build("${repo_url}")
              echo 2
              sh "docker tag ${repo_url}:latest ${env.Image_Vesrion}" 
              echo 3
              sh "docker push ${Image_Vesrion}"
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

def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
pipeline {
  agent any
    stages{
      stage('repoUrl'){
        steps{
          checkout scm
          }
        }

      stage('Create & Push DockerImage') {
        steps {
          script{
            def repo_url = makeSureECRExists(ecrRepoName, region)
            echo repo_url
            def ImageTag = "${repo_url}:${version}"
            sh "\$(aws ecr get-login --no-include-email --region ${region})"
            sh "docker build . -t ${ImageTag} "
            sh "docker push ${ImageTag}"
          }
        }
      }
    }
  }

def makeSureECRExists(ecrRepoName, region){
  try{
    def repoUrl = sh script:"aws ecr create-repository --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
    echo "${repoUrl}"
    return repoUrl
  }catch(Exception ex){
    echo "INFO repository already exists"
  }finally{
  def repoUrl = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
  return repoUrl
  }
}

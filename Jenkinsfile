def ecrRepoName = "ecr_docker_repository"
def version = "latest"
def region = "us-west-2"
pipeline {
    agent any
    stages {
        stage('Git Checkout') {
            steps {
            checkout(scm)
            }
        }
        stage('Create & Push DockerImage') {
            steps {
            script{
              def repo_url = makeSureECRExists(ecrRepoName, region)
              echo repo_url
            sh "\$(aws ecr get-login --no-include-email --region ${region})"
            sh "docker build -t ecr_docker_repository ."
            sh "docker tag ecr_docker_repository:latest ${repo_url}:${version}"
            sh "docker push ${repo_url}:${version}"
            }
          }
        }
    }
}

def makeSureECRExists(ecrRepoName, region){
  try{
    def repoUrl = sh script:"aws ecr create-repository --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'",  returnStdout: true
    return repoUrl
  }catch(error){
    echo "INFO repository already exists"
    def repoUrl = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
    return repoUrl
  }
}

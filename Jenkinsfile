def ecrRepoName = "ecr_docker_repository"
def version = "latest"
def region = "us-west-2"
pipeline {
    agent any
    stages {
        stage('Create ECR') {
            steps {
            checkout(scm)
            }
        }
        stage('Create ECR') {
            steps {
              def repo_url = makeSureECRExists(ecrRepoName, region)
            }
        }
        stage('Create & Push DockerImage') {
            steps {
            sh "\$(aws ecr get-login --no-include-email --region ${region})"
            sh "docker build -t ecr_docker_repository ."
            sh "docker tag ecr_docker_repository:latest ${repo_url}:${version}"
            sh "docker push ${repo_url}:${version}"
            }
        }
    }
}

def makeSureECRExists(ecrRepoName, region){
  try{
    def repoDetails = sh script:"aws ecr create-repository --repository-name ${ecrRepoName} --region ${region}",  returnStdout: true
    return repoDetails['repository']['repositoryUri'];
  }catch{
    echo "INFO repository already exists"
    def repoDetails = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region}", returnStdout: true
    return repoDetails['repositories'][0]['repositoryUri'];
  }
}

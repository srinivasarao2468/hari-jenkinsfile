def ecrRepoName = "srinivas"
def version = "1.0"
def region = "us-west-2"
pipeline {
    agent any
    stages{
        stage('repoUrl'){
            steps{
            checkout scm
            makeSureECRExists(ecrRepoName, region)
            }
        }
<<<<<<< HEAD
        stage('Create & Push DockerImage') {
            steps {
            script{
              def repo_url = makeSureECRExists(ecrRepoName, region)
              echo repo_url
              def ImageTag = "${repo_url}:${version}"
            sh "\$(aws ecr get-login --no-include-email --region ${region})"
            sh "docker build -t ${ImageTag} ."
            sh "docker push ${ImageTag}"
            }
          }
        }
=======
>>>>>>> fec47d00113bb011aaf6733150cb660d45a3958b
    }
}
def makeSureECRExists(ecrRepoName, region){
  try{
    def repoUrl = sh (returnStdout: true, script: "aws ecr create-repository --repository-name srinivas --region us-west-2 --output text | awk '{print \$NF}'").trim()
    echo "${repoUrl}" 
    return repoUrl
<<<<<<< HEAD
  }catch(Exception ex){
    echo "INFO repository already exists"
  }finally{
  def repoUrl = sh script:"aws ecr describe-repositories --repository-name ${ecrRepoName} --region ${region} --output text | awk '{print \$NF}'", returnStdout: true
  return repoUrl
=======
  } catch(Exception ex){
    echo "INFO repository already exists"
  }finally {
    def repoUrl = sh (returnStdout: true, script: "aws ecr describe-repositories --repository-name srinivas --region us-west-2 --output text | awk '{print \$NF}'").trim()
    echo "${repoUrl}"
    return repoUrl
>>>>>>> fec47d00113bb011aaf6733150cb660d45a3958b
  }
}

pipeline {
  agent any
  stages {
    stage('repoUrl') {
      parallel {
        stage('repoUrl') {
          steps {
            script {
              def commitHash = checkout(scm).GIT_COMMIT
              echo "${commitHash}"
            }

          }
        }
        stage('') {
          steps {
            echo 'Hari'
          }
        }
      }
    }
    stage('Create & Push DockerImage') {
      steps {
        script {
          sh "\$(aws ecr get-login --no-include-email --region ${region})"
          echo "1"
          sh "docker build . -t ${env.Image_Vesrion}"
          echo "2"
          //sh "docker tag '${env.repo_url}:latest' ${env.Image_Vesrion}"
          echo "3"
          sh "docker push ${env.Image_Vesrion}"
        }

      }
    }
  }
  environment {
    repo_url = makeSureECRExists(ecrRepoName, region).trim()
    Image_Vesrion = "${env.repo_url}:${env.BUILD_NUMBER}"
  }
}
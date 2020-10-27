pipeline{
  agent any
  environment {
    registryBlue = "sumitmann/devops_capstone_blue"
    registryGreen = "sumitmann/devops_capstone_green"
    registryCredential = 'dockercredentials'
    dockerTag = getDockerTag()
  }
  stages{
    stage('Lint HTML'){
      steps{
            sh 'tidy -q -e Blue/index.html'
            sh 'tidy -q -e Green/index.html'
      }
    }
    stage("Lint Dockerfile"){
      agent{
          docker{
              image 'hadolint/hadolint:latest-debian'
          }
        }
      steps {
          sh 'hadolint Blue/Dockerfile.blue | tee -a hadolint_lint.txt'
          sh '''
              lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
              if [ "$lintErrors" -gt "0" ]; then
                  echo "Check Error"
                  cat hadolint_lint.txt
                  exit 1
              else
                  echo "No Error"
              fi
          '''
          sh 'hadolint Green/Dockerfile.green | tee -a hadolint_lint.txt'
          sh '''
              lintErrors=$(stat --printf="%s"  hadolint_lint.txt)
              if [ "$lintErrors" -gt "0" ]; then
                  echo "Check Error"
                  cat hadolint_lint.txt
                  exit 1
              else
                  echo "No Error"
              fi
          '''
          }
        }
    stage('Build Docker Image'){
      steps{
        sh "docker build -f Blue/Dockerfile.blue Blue -t ${registryBlue}:${dockerTag}"
        sh "docker build -f Green/Dockerfile.green Green -t ${registryGreen}:${dockerTag}"
      }
    }
    stage('Push Docker Image'){
      steps{
        script{
          docker.withRegistry('', registryCredential) {
            sh "docker push ${registryBlue}:${dockerTag}"
            sh "echo docker tag ${registryBlue}:${dockerTag}"
            sh "docker push ${registryGreen}:${dockerTag}"
          }
        }
      }
    }
  }    
}


def getDockerTag() {  
  def tag = sh script: 'git rev-parse --short=7 HEAD', returnStdout: true
  return tag
}
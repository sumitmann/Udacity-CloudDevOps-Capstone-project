pipeline{
  agent any
  stages{
    stage('Lint HTML'){
			steps{
        sh 'tidy -q -e Blue/index.html'
        sh 'tidy -q -e Green/index.html'
      }
		}
     stage('Upload to AWS'){
        steps{
          withAWS(region:'eu-west-1',credentials:'udacity-capstone'){
            s3Upload(file:'Blue/index.html', bucket:'my-udacity-capstone-project-bucket', path:'index.html')
          }
        }
     }
  }
}

pipeline{
  agent any
  stages{
    stage('Lint HTML'){
			steps{
				sh 'tidy -q -e *.html'
			}
		}
     stage('Upload to AWS'){
        steps{
          withAWS(region:'eu-west-1',credentials:'udacity-capstone'){
            s3Upload(file:'index.html', bucket:'my-udacity-capstone-project-bucket', path:'index.html')
          }
        }
     }
  }
}

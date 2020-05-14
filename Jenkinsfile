pipeline {
     agent any
     stages {
         stage('Build') {
             steps {
                 sh 'echo "Hello World"'
                 sh '''
                     echo "Multiline shell steps works too"
                     ls -lah
                 '''
             }
         }
	 stage('Lint HTML') {
              steps {
                  sh 'tidy -q -e *.html'
              }
         }
         
	stage('Scan') {
        steps{
            aquaMicroscanner imageName: '', notCompliesCmd: 'exit 4', onDisallowed: 'fail', outputFormat:'html'
        	} 
	}
	     
         stage('Upload to AWS') {
              steps {
                  
		   withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'Jenkins-install-user', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
                        {
                              s3Upload(pathStyleAccessEnabled: true, payloadSigningEnabled: true, file:'index.html', bucket:'static-jenkins-pipeline1989')
                        }
                     
                  }
              }
         }
     }


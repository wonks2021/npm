pipeline {
agent {
    node {
        label 'slave1'
}}
triggers {
    githubPush()    
    }
environment {
npm = '/usr/local/bin/npm'
}
stages {
stage('Cleancache') {
      steps {
            sh 'rm -rf /home/ubuntu/jenkins/workspace/npm101/*'
       }
    }
stage ('Checkout') {
            steps {
                //this is a comment 
                git credentialsId: 'userId', url: ': 'master'
            }
}
stage ('Restore PACKAGES') {     
         steps {
             sh "export PATH=/usr/local/bin/npm:$PATH"
			 sh "npm install"
          }
        }
stage('Clean') {
      steps {
            sh ' npm cache clean --force'
       }
    }
stage('Build') {
     steps {
            sh 'npm build'
      }
   }

    	stage ('Server'){
            steps {
               rtServer (
                 id: "Artifactory",
                 url: 'https://projectnuget.jfrog.io/artifactory',
                 username: 'admin1',
                  password: 'Admin@123',
                  bypassProxy: true,
                   timeout: 300
                        )
            }
        }
        stage('Upload'){
            steps{
                rtUpload (
                 serverId:"Artifactory" ,
                  spec: '''{
                   "files": [
                      {
                      "pattern": "dist/*",
                      "target": "npm101-npm-local"
                      }
                            ]
                           }''',
                        )
            }
        }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "Artifactory"
                )
            }
        }
 }
}

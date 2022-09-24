pipeline {
   agent any

   stages{
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            bat '''docker images -a'''
          bat '''
               cd azure-vote/
               docker images -a
               docker build -t jenkins-pipeline .
               docker images -a
               cd ..'''
         }
      }

      stage('Start test app') {
         steps {
            echo "$STAGE_NAME"
            
            bat '''
               docker-compose up -d
               PowerShell ./scripts/test_container.ps1
            '''
         }

         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }

      stage('Run Tests') {
         steps {
            bat '''
               pytest ./tests/test_sample.py
            '''
         }
      }

      stage('Stop test app') {
         steps {
            bat'''
               docker-compose down
            '''
         }
      }

      stage("Push Container"){
         steps{
            echo "Workspace is : $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
               script{
                  docker.withRegistry("https //index.docker.io/v1/ ","dockerHub"){
                     def image = docker.build("blackdentech/jenkins-course:latest")
                     image.Push()
                  }
               }
            }
         }
      }


   }
}
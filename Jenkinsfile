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
            echo "change"
            
            bat '''
               docker-compose up -d
            '''
            sh './scripts/test_container.ps1'
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

   }
}
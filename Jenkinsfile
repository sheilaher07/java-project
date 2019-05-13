pipeline {
   agent {label 'linux'}
   stages {
      stage('Unit Tests') {
         steps {
            sh 'ant -f test.xml -v'
            junit 'reports/result.xml'
         }
      }
      stage('Build') {
         steps {
            sh 'ant -f build.xml -v'
         }
      }
      stage('Deploy') {
         steps {
            sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://assign-10/'
         }
      }
      stage('Report') {
         steps {
            withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-credential-assign10', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
            }
         }
      }
   }
}

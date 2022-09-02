pipeline {
    agent any
    stages {
        stage('Git clone') {
            steps {
            git 'https://github.com/abjal1/codedeploy.git'
            }
        }
        stage('Build') {
            steps {
                sh 'cat index.html'
            }
        }
        stage('zip') {
            steps {
                sh 'zip test.zip index.html appspec.yml ./Scripts/start_server.sh ./Scripts/stop_server.sh'
            }
        }
        stage('Push to s3') {
            steps {
                sh 'aws s3 cp test.zip s3://abjals3/test.zip'
            }
        }
        stage('deploy app') {
            steps {
                createDeployment(
                  s3Bucket: 'abjals3',
                  s3Key: 'test.zip',
                  s3BundleType: 'zip', // [Valid values: tar | tgz | zip | YAML | JSON]
                  applicationName: 'test',
                  deploymentGroupName: 'test-deploygroup',
                  deploymentConfigName: 'CodeDeployDefault.AllAtOnce',
                  description: 'Test deploy',
                  waitForCompletion: 'true',
                  //Optional values 
                  ignoreApplicationStopFailures: 'false',
                  fileExistsBehavior: 'OVERWRITE'// [Valid values: DISALLOW, OVERWRITE, RETAIN]
                )
            }
        }
    }
}

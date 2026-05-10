pipeline{
    agent any
    environment{
        BUCK = "aws-s3-3107"
    }
    stages{
        stage("clone"){
            steps{
                echo "cloning"
                sh 'aws s3 ls'
                git branch: 'main', url: 'https://github.com/pushkar31/staticweb.git'
            }
        }
            stage("check"){
            steps{
                echo "checking if bucket is there or not"
                sh '''
                if aws s3 ls s3://${BUCK}; then 
                    echo "bucket already exist"
                else
                    echo "creating bucket...."
                    aws s3 mb s3://${BUCK}
                    
                    fi                
                    '''
            }
            }
            
            stage("deploy"){
            steps{
                echo "sync to s3 bucket"
                sh 'aws s3 sync . s3://${BUCK}/'
            }
        }
    }
    post{
        always{
            echo "it will run always"
        }
        failure{
            echo "pipeline got failed"
        }
        success {
            echo "creating the artifact"
            sh 'tar -cvf staticweb.tar index.html'
            
        }
    }
}

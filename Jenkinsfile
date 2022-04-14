pipeline {
    agent any

    stages{
        stage('deploy to S3'){
            steps{
                sh 'echo \'{"Version": "2012-10-17","Statement": [{"Sid": "PublicReadGetObject","Effect": "Allow","Principal": "*","Action": "s3:GetObject","Resource": "arn:aws:s3:::\${bucketname}/*"}]}\' > /tmp/bucket_policy.json'
                sh 'aws s3api create-bucket --bucket \${bucketname} --region ap-south-1  --create-bucket-configuration LocationConstraint=ap-south-1'
                sh 'aws s3api put-bucket-policy --bucket \${bucketname} --policy file:///tmp/bucket_policy.json'
                sh 'aws s3 cp public/index.html s3://\${bucketname}'
                //sh 'aws s3api put-object-acl --bucket jenkins-demo-mind --key index.html --acl public-read'
                sh 'aws s3 cp public/error.html s3://\${bucketname}'
                //sh 'aws s3api put-object-acl --bucket jenkins-demo-mind --key error.html --acl public-read'
                sh 'aws s3 website s3://\${bucketname}/ --index-document index.html --error-document error.html'
            }
        }
    }
    post{
        always{
            cleanWs disableDeferredWipeout: true, deleteDirs: true
        }
    }

}
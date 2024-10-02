Deploy a static website using githubactions and s3 bucket
Step 1: Set Up Your S3 Bucket

Create s3 bucket: /gayatri-bucket-githubaction/ Enable static website hosting : index.html Add bucket policy:

{
___"Version": "2012-10-17",
___"Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*"
    }
  ]
}
Step 2: Configure AWS Credentials

Attach the AmazonS3FullAccess policy to the user. Add Secrets to GitHub: AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY

Step 3: Create GitHub Actions Workflow

create a .github/workflows directory and add deploy.yml file

name: Deploy Static Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1  # Change to your bucket's region

    - name: Sync files to S3
      run:
        aws s3 sync . s3://YOUR_BUCKET_NAME --delete
        
The file will automatically deploy your site on s3 bucket. Check your website is running or not by copying the link provided in the bucket properties.
![image](https://github.com/user-attachments/assets/ef7a33c3-a181-4e5b-ab8b-5f7c9f4028e3)



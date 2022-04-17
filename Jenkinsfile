pipeline {
agent any
environment {
AWS_ACCOUNT_ID="437144803032"
AWS_DEFAULT_REGION="us-east-2"
IMAGE_REPO_NAME="nodeimage-aswath"
  IMAGE_TAG="${BUILD_NUMBER}"
REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
}
stages {
stage('Checkout') {
steps {
git branch: 'master',
credentialsId: 'Github-aswath',
url: 'https://github.com/aswath-pt/node-js-ecr.git'
}
}
stage('Logging into AWS ECR') {
 steps {
 script {
 sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
}
}
}stage('Building image') {
steps {
script {
dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
sh "docker tag ${IMAGE_REPO_NAME}:${IMAGE_TAG} ${REPOSITORY_URI}:$IMAGE_TAG"
}
}
}
stage('Pushing to ECR') {
steps {
script {
docker.withRegistry('https://437144803032.dkr.ecr.us-east-2.amazonaws.com', 'ecr:ap-south-1:demo-ecr-credentials') {
sh "echo ${REPOSITORY_URI}"
sh "echo ${IMAGE_TAG}"
docker.image('437144803032.dkr.ecr.us-east-2.amazonaws.com/nodeimage-aswath').push('${IMAGE_TAG}')
}
}
}
}
}
}


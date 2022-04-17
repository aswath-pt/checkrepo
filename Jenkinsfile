pipeline {
agent any
environment {
AWS_ACCOUNT_ID="340193729058"
AWS_DEFAULT_REGION="ap-south-1"
IMAGE_REPO_NAME="my-ecr-repo1"
IMAGE_TAG="latest"
REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
}
stages {
stage('Checkout') {
steps {
git branch: 'master',
credentialsId: '8c798652-c075-40da-b0af-78422abcb159',
url: 'https://github.com/cloudtechner/nodejs-project.git'
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
docker.withRegistry('https://340193729058.dkr.ecr.ap-south-1.amazonaws.com', 'ecr:ap-south-1:demo-ecr-credentials') {
sh "echo ${REPOSITORY_URI}"
sh "echo ${IMAGE_TAG}"
docker.image('340193729058.dkr.ecr.ap-south-1.amazonaws.com/my-ecr-repo1').push('latest')
}
}
}
}
}
}


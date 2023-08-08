pipeline {
    agent any
    tools {
        maven "mavenTest"
    }
    environment {
        workspace = '/var/lib/jenkins/workspace'
        deploy_folder = "${workspace}/deploy"
        pet_jar = "petclinic-${env.BUILD_NUMBER}.jar"
        ECR_PATH = '${ECR_URL}'
        IMAGE_NAME = 'petclinic'
        REGION = 'ap-northeast-2'
    }
    
    stages {
        stage('git clone') {
            steps {
                checkout scm
            }
        }
        stage ('Build Maven') {
            steps {
                script {
                    sh './mvnw package -X'
                    sh """
                    if [ ! -d "${deploy_folder}" ]; then
                        mkdir -p "${deploy_folder}"
                    fi
                    """
                    sh "cp ${workspace}/${env.JOB_NAME}/target/*.jar ${deploy_folder}/${pet_jar}"
                }
            }
        }
        stage("Docker Build and Push") {
            steps {
                script {
                    sh """
                    #!/bin/bash
                    cat > Dockerfile <<-EOF
FROM openjdk:22-jdk-slim
ADD ${deploy_folder}/${pet_jar} /home/${pet_jar}
CMD ["nohup", "java", "-jar", "-Dspring.profiles.active='mysql'", "/home/${pet_jar}.jar"]
EOF"""
                    
                    docker.withRegistry("https://${ECR_PATH}", "ecr:${REGION}:shg7897@naver.com") {
                        image = docker.build("${ECR_PATH}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                        image.push()
                    }
                }
            }
        }
    }
}

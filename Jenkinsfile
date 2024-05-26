pipeline {
    agent any

    stages {
        stage('build-docker-image') {
            steps {
                build_docker_image()
            }
        }
        stage('deploy-to-dev') {
            steps {
                deploy("dev")
            }
        }
        stage('tests-on-dev') {
            steps {
                run_api_tests("dev")
            }
        }
        stage('deploy-to-stg') {
            steps {
                deploy("stg")
            }
        }
        stage('tests-on-stg') {
            steps {
                run_api_tests("stg")
            }
        }
        stage('deploy-to-prod') {
            steps {
                deploy("prod")
            }
        }
        stage('tests-on-prod') {
            steps {
                run_api_tests("prod")
            }
        }
    }
}

def build_docker_image(){
    sh "ls"
    echo "Building docker image.."
    sh "docker build --no-cache -t jekaterina2021/python-greetings-app:latest ."

    echo "Pushing docker image to docker registry.."
    sh "docker push jekaterina2021/python-greetings-app:latest"
}

def deploy(String environment){
    echo "Deploying Python microservice to ${environment} environment.."

    sh "docker pull jekaterina2021/python-greetings-app:latest"
    sh "docker-compose stop greetings-app-${environment}"
    sh "docker-compose rm greetings-app-${environment}"
    sh "docker-compose up -d greetings-app-${environment}"
}

def run_api_tests(String environment){
    echo "API tests for Python microservice on ${environment} environment.."

    sh "docker pull jekaterina2021/api-tests:latest"
    sh "docker run --network=host --rm jekaterina2021/api-tests:latest run greetings greetings_${environment}"
}
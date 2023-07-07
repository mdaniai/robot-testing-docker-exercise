pipeline {
    agent any
    environment {
        GIT_URL = 'git@github.com:mdaniai/robot-testing-docker-exercise.git'
        BRANCH = 'main'
        ROBOT = '/home/user/.local/bin/robot'
        CHANNEL = '#jenkins'
        IMAGE = 'my-robot-test'
        CONTAINER = 'my-robot-test-app'
        DOCKER_APP = '/usr/bin/docker'
    }
    stages {
        stage('Cleaning up') {
            steps {
                echo 'Cleaning up'
                sh "${DOCKER_APP} rm -f ${CONTAINER} || true"
            }
        }

        stage('Clone') {
            steps {
                echo 'Clone'
                git branch: "${BRANCH}", url: "${GIT_URL}"
            }
        }

        stage('Build') {
            steps {
                echo 'Build'
                sh "${DOCKER_APP} build -t ${IMAGE} ."
            }
        }

        stage('Run') {
            steps {
                echo 'Run Test'
                sh "${DOCKER_APP} run --rm ${IMAGE}"
            }
        }
    }
    post {
        always {
            echo 'Always run'
        }
        success {
            echo 'Run only on success build'
            slackSend(channel: "${CHANNEL}", message: "Build deployed successfully")
        }
        failure {
            echo 'Run only on failed build'
            slackSend(channel: "${CHANNEL}", message: "Build failed to deploy")
        }
    }
}
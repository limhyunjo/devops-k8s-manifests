pipeline {
    agent any
    parameters {
        string(name: 'DOCKER_IMAGE_VERSION', defaultValue: '', description: 'First parameter')
    }
    stages {
        stage('Update deploy.yaml') {
            steps {
                // Jenkins 파이프라인에서 작업 디렉토리를 변경할 때 사용한다.
                dir('university-api') {
                    echo "${params.DOCKER_IMAGE_VERSION}"
                    sh 'pwd'
                    sh 'ls -al'
                    sh 'git checkout main'
                    sh "sed -i 's|limhyunjo/university-api:.*|limhyunjo/university-api:${params.DOCKER_IMAGE_VERSION}|g' deploy.yaml"
                    sh 'cat deploy.yaml'
                }
            }
        }

        stage('Commit & Push') {
            steps {
                sh 'git config --list'
                sh 'git config user.name "limhyunjo"'
                sh 'git config user.email "limhyunjo@gmail.com"'
                sh 'git config --list'
                sh 'git add .'
                sh "git commit -m 'Update Image Version ${params.DOCKER_IMAGE_VERSION}'"
                sh 'git status'
                sshagent(['github-manifests-access-key']) {
                    sh 'git push'
                }
            }
        }
    }
}
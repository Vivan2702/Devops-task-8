pipeline {
    agent { dockerfile true }

    environment {
        DOCKERHUB_USERNAME = credentials('vivan27')
        DOCKERHUB_PASSWORD = credentials('dckr_pat_ASPLhdr2kK-J7OOvKO_gGDsC8eI')
    }

    stages {
        stage('Build and Test') {
            steps {
                script {
                    def isMainBranch = env.GIT_BRANCH == 'origin/main'
                    def isReleaseTag = env.GIT_BRANCH.startsWith('origin/tags/v')

                    sh "docker build -t vivan27/devops-task-8:${env.GIT_COMMIT} ."
                    sh "docker run --rm vivan27/devops-task-8:${env.GIT_COMMIT} python manage.py test"

                    if (isMainBranch || isReleaseTag) {
                        sh "docker login -u ${DOCKERHUB_USERNAME} -p ${DOCKERHUB_PASSWORD}"
                        sh "docker push vivan27/devops-task-8:${env.GIT_COMMIT}"
                    }
                }
            }
        }
    }
}
currentBuild.result = "SUCCESS"
ansiColor('xterm') {
    node {

        def app
        def dockerRegistry = 'https://registry.hub.docker.com'
        def appVersion = ''
        def dockerImage = 'dniel/blogr-www'

        stage('Prepare) {
            deleteDir()
            checkout scm
            def gitShortCommit = sh([
                    returnStdout: true,
                    script      : 'git rev-parse --short HEAD'
            ]).trim()

            def gitCommitTime = sh([
                    returnStdout: true,
                    script      : 'git log -1 --pretty=format:%ct|date +"%m%d%Y-%H%M"'
            ]).trim()
            appVersion = "${gitCommitTime}-${gitShortCommit}"
        }

        stage('Build and Push Docker Image') {
            container('docker'){
              withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh "docker login -u ${USERNAME} -p ${PASSWORD}"
                sh "docker build -t ${dockerImage} ."
                sh "docker tag ${dockerImage} ${dockerImage}:${appVersion}"
                sh "docker push ${dockerImage}"
              }
            }
        }
    }
}

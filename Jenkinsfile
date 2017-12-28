currentBuild.result = "SUCCESS"
ansiColor('xterm') {
    node {


        def app
        def dockerRegistry = 'https://registry.hub.docker.com'
        def appVersion = ''

        stage('Clone repository') {
            deleteDir()
            checkout scm
        }

        stage('Build image') {
            def gitShortCommit = sh([
                    returnStdout: true,
                    script      : 'git rev-parse --short HEAD'
            ]).trim()

            def gitCommitTime = sh([
                    returnStdout: true,
                    script      : 'git log -1 --pretty=format:%ct|date +"%m%d%Y-%H%M"'
            ]).trim()

            appVersion = gitCommitTime() + '-' + gitShortCommit()
            app = docker.build("dniel/blogr-www")
        }

        stage('Test image') {
            app.inside {
                sh 'echo "Tests passed"'
            }
        }

        stage('Push image') {
            docker.withRegistry(dockerRegistry, 'docker-hub-credentials') {
                app.push(appVersion)
                app.push("latest")
            }
        }
    }
}
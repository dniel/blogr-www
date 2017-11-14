import java.text.SimpleDateFormat

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
            mvn("-B package")
            appVersion = mavenAppVersion()
            app = docker.build("dniel/api-posts")
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

def mavenAppVersion() {
    def appVersion
    appVersion = sh([
            returnStdout: true,
            script      : 'grep version target/maven-archiver/pom.properties | awk -F= \'{ print $2 }\''
    ]).trim()
    appVersion
}

def rancher() {
    return load("rancher.groovy");
}

def mvn(arguments) {
    def gitShortCommit = sh([
            returnStdout: true,
            script      : 'git rev-parse --short HEAD'
    ]).trim()

    def gitCommitTime = sh([
            returnStdout: true,
            script      : 'git log -1 --pretty=format:%ct|date +"%m%d%Y-%H%M"'
    ]).trim()

    withMaven(maven: 'M35') {
        sh "mvn ${arguments} -Dsha1=${gitShortCommit} -Dchangelist=${gitCommitTime}"
    }
}
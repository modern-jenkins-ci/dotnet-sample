node('docker') {
    stage('Get Latest Code') {
        cleanWs()
        checkout scm
    }

    stage('Pull .Net Sample Code') {
        dir('dotnet-docker-samples') {
            def gitInfo = git url: 'https://github.com/dotnet/dotnet-docker-samples.git', branch: 'master'

            // set the git commit info on the environment
            if(gitInfo.GIT_COMMIT) {
                env.GIT_COMMIT = gitInfo.GIT_COMMIT
            }
        }
    }

    stage('Build Docker Image') {
        def builder
        dir('dotnet-docker-samples/aspnetapp') {
            if(env.http_proxy) {
                builder = docker.build(
                    "modern-jenkins/aspnetapp:${env.BUILD_NUMBER}",
                    "--build-arg http_proxy=${env.http_proxy} --build-arg https_proxy=${env.https_proxy} ."
                )
            } else {
                builder = docker.build("modern-jenkins/aspnetapp:${env.BUILD_NUMBER}")
            }
        }
    }
}
pipeline {
    agent any

    environment {
        RELEASE_VERSION = sh(
            script: 'echo $BUILD_NUMBER',
            returnStdout: true
        ).trim()
    }

    stages {
        stage('Publish - Secret Text (PAT)') {
            steps {
                createGitHubRelease(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag: "${env.RELEASE_VERSION}-pat",
                    bodyText: 'Testing PAT credential path.',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-pat",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/random_binary_file.bin"],
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }

        stage('Publish - GitHub App') {
            steps {
                createGitHubRelease(
                    credentialId: 'github-app',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag: "${env.RELEASE_VERSION}-app",
                    bodyText: 'Testing GitHub App credential path.',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'github-app',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-app",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/random_binary_file.bin"],
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }
    }
}

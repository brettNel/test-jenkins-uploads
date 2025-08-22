pipeline {
    agent any
    
    environment {
        RELEASE_VERSION = sh(
            script: 'echo $BUILD_NUMBER',
            returnStdout: true
        ).trim()
    }
    stages {
        stage('Publish') { 
            steps {
                createGitHubRelease(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag:  env.RELEASE_VERSION,
                    bodyText: 'testing publish',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName:  env.RELEASE_VERSION,
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/random_binary_file.bin"],
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }
    }
}

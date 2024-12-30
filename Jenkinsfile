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
                    credentialId: 'DevOpsToken',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag:  env.RELEASE_VERSION,
                    bodyText: 'testing publish',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'DevOpsToken',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName:  env.RELEASE_VERSION,
                    uploadAssets: [
                        [filePath: 'missingfile.hex'],
                        [filePath: 'random_binary_file.bin'],
                        [filePath: 'small_txt_file.txt']
                    ]
                )
            }
        }
    }
}

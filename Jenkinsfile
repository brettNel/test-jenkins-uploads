pipeline {
    agent any
    stages {
        stage('Publish') { 
            steps {
                createGitHubRelease(
                    credentialId: 'DevOpsToken',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag:  '$BUILD_NUMBER',
                    bodyText: 'testing publish',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'DevOpsToken',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName:  '$BUILD_NUMBER',
                    uploadAssets: [
                        [filePath: 'random_binary_file.bin'],
                        [filePath: 'small_txt_file.txt']
                    ]
                )
            }
        }
    }
}

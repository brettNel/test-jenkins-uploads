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
                        [filePath: "${WORKSPACE}/random_binary_file.bin"]
                    ]
                )
                // Upload a second asset to the same existing release
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-pat",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }

        stage('Publish - Overwrite Assets') {
            steps {
                createGitHubRelease(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag: "${env.RELEASE_VERSION}-overwrite",
                    bodyText: 'Testing asset overwrite.',
                )
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-overwrite",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
                // Upload again with overwrite: true — should replace the existing asset
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-overwrite",
                    overwrite: true,
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }

        stage('Publish - Draft with Assets') {
            steps {
                createGitHubRelease(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag: "${env.RELEASE_VERSION}-draft",
                    bodyText: 'Testing draft release asset upload.',
                    draft: true,
                )
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-draft",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/random_binary_file.bin"]
                    ]
                )
                // Upload a second asset to the same existing draft
                uploadGithubReleaseAsset(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    tagName: "${env.RELEASE_VERSION}-draft",
                    uploadAssets: [
                        [filePath: "${WORKSPACE}/small_txt_file.txt"]
                    ]
                )
            }
        }

        stage('Publish - Generated Release Notes') {
            steps {
                script {
                    def generated = createGitHubRelease(
                        credentialId: 'BrettNel2',
                        repository: 'brettNel/test-jenkins-uploads',
                        commitish: 'main',
                        tag: "${env.RELEASE_VERSION}-notes",
                        generateReleaseNotes: true,
                    )
                    echo "Generated notes:\n${generated.body}"
                    if (!generated.body?.trim()) {
                        error('Expected GitHub to generate a release body')
                    }
                    if (generated.name != "${env.RELEASE_VERSION}-notes") {
                        error("Expected release name to default to the tag, got '${generated.name}'")
                    }

                    // bodyText should be prepended to the generated notes
                    def prepended = createGitHubRelease(
                        credentialId: 'BrettNel2',
                        repository: 'brettNel/test-jenkins-uploads',
                        commitish: 'main',
                        tag: "${env.RELEASE_VERSION}-notes-body",
                        bodyText: 'Testing generated notes with bodyText.',
                        generateReleaseNotes: true,
                    )
                    echo "Generated notes with bodyText:\n${prepended.body}"
                    if (!prepended.body?.startsWith('Testing generated notes with bodyText.')
                            || prepended.body.trim() == 'Testing generated notes with bodyText.') {
                        error('Expected bodyText followed by generated notes')
                    }
                }
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

        stage('Create Release - Existing Tag (expect failure)') {
            steps {
                createGitHubRelease(
                    credentialId: 'BrettNel2',
                    repository: 'brettNel/test-jenkins-uploads',
                    commitish: 'main',
                    tag: "${env.RELEASE_VERSION}-pat",
                    bodyText: 'This should fail - tag already exists.',
                )
            }
        }
    }
}

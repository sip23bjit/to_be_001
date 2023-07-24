pipeline {
    agent any
    tools {
        maven 'MAVEN'
    }
    // triggers {
    //     pollSCM 'H/5 * * * *'
    // }
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/sip23bjit/project_code_006.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(configName: 'polak', transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            excludes: '',
                            execCommand: './lms.sh && sudo systemctl restart javarunner.service',
                            execTimeout: 120000,
                            flatten: false,
                            makeEmptyDirs: false,
                            noDefaultExcludes: false,
                            patternSeparator: '[, ]+',
                            remoteDirectory: '',
                            remoteDirectorySDF: false,
                            removePrefix: 'target/',
                            sourceFiles: 'target/*jar'
                        )
                    ],
                    usePromotionTimestamp: false,
                    useWorkspaceInPromotion: false,
                    verbose: false
                    )
                ])
            }
        }
    }
}

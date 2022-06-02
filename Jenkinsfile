pipeline {
    agent { label 'vicont' }

    parameters {
        gitParameter (  branch: '',
                        branchFilter: '.*',
                        defaultValue: 'main',
                        description: '',
                        name: 'BACKEND_TAG',
                        quickFilterEnabled: true,
                        selectedValue: 'NONE',
                        sortMode: 'NONE',
                        tagFilter: '*',
                        type: 'PT_TAG',
                        useRepository: 'git@github.com:manjestik/test2.git')
        gitParameter (  branch: '',
                        branchFilter: '.*',
                        defaultValue: 'main',
                        description: '',
                        name: 'FRONTEND_TAG',
                        quickFilterEnabled: true,
                        selectedValue: 'NONE',
                        sortMode: 'NONE',
                        tagFilter: '*',
                        type: 'PT_TAG',
                        useRepository: 'git@github.com:manjestik/test3.git')
    }

    stages {
        stage('Delete workspace before build starts') {
            steps {
                echo 'Deleting workspace'
                deleteDir()
            }
        }
        stage('Env print') {
            steps {
                sh '''
                    echo Selected Tag name: $BACKEND_TAG
                    echo Selected Tag name: $FRONTEND_TAG
                '''
            }
        }
        stage('Checkout') {
            parallel {
                stage('TAG') {
                    steps{
                        checkout(
                            [$class: 'GitSCM',
                            branches: [[name: "${params.TAG}"]],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [],
                            submoduleCfg: [], userRemoteConfigs:
                            [[credentialsId: 'manjestik_github',
                            url: 'git@github.com:manjestik/test2.git']]]
                        )

                        checkout(
                            [$class: 'GitSCM',
                            branches: [[name: "${params.TAG}"]],
                            doGenerateSubmoduleConfigurations: false,
                            extensions: [],
                            submoduleCfg: [], userRemoteConfigs:
                            [[credentialsId: 'manjestik_github',
                            url: 'git@github.com:manjestik/test3.git']]]
                        )
                    }
                }
            }
        }
        stage('Ls work dir') {
            steps {
                sh '''
                    ls -la
                    cat server.py
                '''
            }
        }

    }
}

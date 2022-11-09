pipeline {
    agent any
    stages {
        stage('Stage 1') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                sh "curl ipinfo.io/ip"
            }
            stage('Stage 2') {
                def reframeDir = "./"
                dir("$reframeDir/reframe") {
                    checkout([$class: 'GitSCM', branches: [[name: '*/master']],
                              doGenerateSubmoduleConfigurations: false,
                              extensions: [[$class: 'WipeWorkspace']], submoduleCfg: [],
                              userRemoteConfigs: [[url: 'https://github.com/reframe-hpc/reframe.git']]])

                    def exitStatus = sh(returnStatus: true,
                                        script: """${loginBash}
                                                   ./bootstrap.sh
                                                   export RFM_AUTODETECT_XTHOSTNAME=1
                                                   ./bin/reframe -C ${configFile} --exec-policy=async --save-log-files -r -J account=jenscscs --flex-alloc-nodes=2 -t 'production|benchmark' $changedTestsOption""")
                    sh("exit $exitStatus")
                }
            }
        }
    }
}

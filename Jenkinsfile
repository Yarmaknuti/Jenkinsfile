node {
    currentBuild.result = "SUCCESS"
            stage('Source') {
                git 'https://github.com/Yarmaknuti/gradle.git'
                }
            stage('Checkout') {
checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: true, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/lobsterlele/gradle']]])
                            }
        def gradleHome = tool 'Gradle'
        stage('Building code') {
sh "'${gradleHome}' clean build test "
}
            stage('Testing code') {
                parallel(
                    firstBranch: {
                        sleep 1 },
                                         secondBranch: {
                                                sleep 2
                                             
                                         },
                                         thirdBranch: {
                                             sleep 3
                                             
                                         }
)
}

stage('Triggering child job') {
build job: 'Another jobs'
}

stage('Packaging and Publishing results') {
archiveArtifacts '**/*.jar'
sh 'tar -c -f /$JENKINS_HOME/workspace/Pipeline/pipeline-$BUILD_NUMBER.tar.gz *'
nexusArtifactUploader artifacts: [[artifactId: 'Artifact', classifier: '', file: '$JENKINS_HOME/workspace/Pipeline/pipeline-$BUILD_NUMBER.tar.gz', type: 'jar']], credentialsId: '712f47fe-78e3-4803-8974-ac42c9bacbd1', groupId: 'GroupId', nexusUrl: '172.23.11.47:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'Illia', version: '1'
}
stage('Asking for manual approval') {
timeout(time: 2, unit: "MINUTES") {
input message: 'Approve Deploy?', ok: 'Yes'
}
}
stage ('Deploy'){

sh ('wget http://172.23.11.47:8081/repository/Illia/GroupId/Artifact/1/Artifact-1.jar ')
sh ('tar -xf Artifact-1.jar ')
}
  stage('Email notifications') {
        if ( currentBuild.result == 'SUCCESS') {
            emailext body: 'Job ok', recipientProviders: [culprits()], subject: 'Job', to: 'yarmaknuti@gmail.com'
      
        } else {
             emailext body: 'Job fail ', recipientProviders: [culprits()], subject: 'Job', to: 'yarmaknuti@gmail.com'
        }
    }
}
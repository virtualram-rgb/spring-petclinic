pipeline {
    agent { label 'buildnode'}
    stages {
        stage ('Git clone') {
            steps {
                git url: 'git@github.com:virtualram-rgb/spring-petclinic.git', branch: 'main'
            }
        }
        stage ('build') {
            steps {
                withSonarqubeEnv('SONAR-QUBE'){
                    sh 'mvn clean install sonar:sonar'
                }
            }
        }
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER" ,
                    serverId: 'jfrog_instance',
                    releaseRepo: 'libs-release-local',
                    snapshotRepo: 'libs-snapshot-local'
                )
            }
        }
        stage('exec maven'){
            steps{
                rtmavenRun (
                    timeout(time: 1, unit: 'HOURS') {
                        waitForQualityGate abortPipeline: true
                    }
                )
            }
        }
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
    }
}

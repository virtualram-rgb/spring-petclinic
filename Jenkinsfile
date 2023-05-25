pipeline {
    agent any
    stages {
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virtualram-rgb/spring-petclinic.git', branch: 'main'
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
        stage('Publishtheartifacts'){
            steps{
                rtPublishBuildInfo (
                    serverId: 'jfrog_instance'
                )
            }
        }
    }
}

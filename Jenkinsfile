pipeline {
    agent any
    stages {
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virtualram-rgb/spring-petclinic.git', branch: 'main'
            }
        }
        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: "mvn_1",
                    pom: "pom.xml",
                    goals: "clean install sonar:sonar",
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }
        stage('artifactory'){
            steps{
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "jfrog_instance",
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

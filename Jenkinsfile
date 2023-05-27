pipeline{
    agent { label 'build_node' } 
    stages{
        stage ('Git clone') {
            steps {
                git url: 'https://github.com/virtualram-rgb/spring-petclinic.git', branch: 'main'
            }
        }
        stage('build'){
            steps{
                sh 'mvn package'
            }
        }
        stage('test'){
            steps{
                sh 'mvn sonar:sonar'
            }
        }
        stage('deploytheartifacts'){
            steps{
                sh'mvn deploy'
            }
        }
    }
}

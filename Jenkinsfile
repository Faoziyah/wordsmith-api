pipeline{
    agent any
    tools {
        maven 'maven-3.9.9' 
    }
    stages{
        stage("Clone"){
            steps{
                git branch: 'jenkins', credentialsId: 'git-user-pass', url: 'https://github.com/Faoziyah/wordsmith-api.git'
                sh"ls -l"
                
            }
        }
        stage("Maven test"){
            steps{
                script{
                    sh"mvn test"
                }
            }
        }
        stage("Maven Package"){
            steps{
                sh"mvn clean package"
                sh"ls -l target/"
            }
        }
        
        stage('SonarQube analysis') {
            tools {
                jdk 'jdk11'
            }
            steps {
                withSonarQubeEnv('sonar') {
                     sh 'mvn sonar:sonar'
                }
               // echo "sonar passed"
            }
        }
        stage("Quality Gate") {
            steps {
                 timeout(time: 1, unit: 'HOURS') {
                     waitForQualityGate abortPipeline: true
                 }
                //echo "sonar passed"
            }
        }
         // stage("Upload jar to Nexus"){
         //    steps{
         //        script{
         //            //def componentVersion = getVersion()
         //             nexusArtifactUploader(
         //                 nexusVersion: 'nexus3',
         //                 protocol: 'http',
         //                 nexusUrl: '13.57.34.38:8081',
         //                 groupId: 'com.example',
         //                 version: "1.0-SNAPSHOT",
         //                 repository: 'maven-snapshots',
         //                 credentialsId: 'nexus-creds',
         //                 artifacts: [
         //                     [artifactId: 'words',
         //                    classifier: '',
         //                     file: "${WORKSPACE}/target/words.jar",
         //                     type: 'jar']
         //                 ]
         //             )
         //           // echo "pushed artifact"
         //        }
         //    }
      //  }
    }
}

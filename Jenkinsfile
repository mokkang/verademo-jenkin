pipeline {
   stages {
      stage('Build') {
         script {
            // Get some code from a GitHub repository
            git 'https://github.com/christyson/verademo'
            sh 'mvn package'
         }
      }
      //stage('SCA SC') {
      //   withCredentials([string(credentialsId: 'SRCCLR_API_TOKEN', variable: 'SRCCLR_API_TOKEN')]) {
      //      sh '''
      //         curl -sSL https://download.sourceclear.com/ci.sh | sh
      //      '''
      //    }
      //}
      stage('Veracode Pipeline Scan') {
         // upload and scan
         withCredentials([usernamePassword(credentialsId: 'VeracodeNH', passwordVariable: 'VERACODEKEY', usernameVariable: 'VERACODEID')]) {
            sh '''
              curl -O https://downloads.veracode.com/securityscan/pipeline-scan-LATEST.zip
              unzip -o pipeline-scan-LATEST.zip
              java -jar pipeline-scan.jar -vid $VERACODEID -vkey $VERACODEKEY -f ./target/verademo.war
           '''
         }
      }
      stage('Veracode Sast') {
         // upload and scan
         withCredentials([usernamePassword(credentialsId: 'Veracode', passwordVariable: 'VERACODEKEY', usernameVariable: 'VERACODEID')]) {
            veracode applicationName: '$JOB_NAME', canFailJob: false, createProfile: true, criticality: 'High', fileNamePattern: '', replacementPattern: '', sandboxName: '', scanExcludesPattern: '', scanIncludesPattern: '', scanName: "$BUILD_NUMBER", teams: '', timeout: 60, uploadExcludesPattern: '', uploadIncludesPattern: '**/**.war', useIDkey: true, vid: VERACODEID, vkey: VERACODEKEY, vpassword: '', vuser: ''
         }
      }
   }
}

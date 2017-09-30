node ("master")  {
   // change the project version (don't rely on version from pom.xml)
   env.BN = VersionNumber([
           versionNumberString : '${BUILD_MONTH}.${BUILDS_TODAY}.${BUILD_NUMBER}', 
           projectStartDate : '2017-09-29', 
           versionPrefix : 'v1.'
       ])

   stage('Provision') {
       echo 'Checkout source code from GitHub ...'
       git branch: 'developer', credentialsId: 'Github', url: 'https://github.com/Williamsja0314/SpringMVCDemo.git'

       echo 'Change the project version ...'
       def W_M2_HOME = tool 'maven'
       bat "${W_M2_HOME}\\bin\\mvn versions:set -DnewVersion=$BN -DgenerateBackupPoms=false"
   }
}

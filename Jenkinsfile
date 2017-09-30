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

       echo "Create a new branch with name release_${BN} ..."
       def W_GIT_HOME = tool 'Git'
       bat "${W_GIT_HOME} checkout -b release_${BN}"

       echo 'Stash the project source code ...'
       stash includes: '**', excludes: '**/TestPlan.jmx', name: 'SOURCE_CODE'

   }
}

node ("TestMachine-ut") {
   // we can also use: withEnv(['M2_HOME=/usr/share/maven', 'JAVA_HOME=/usr']) {}
   env.MAVEN_HOME = '/usr/share/maven'
   env.M2_HOME = '/usr/share/maven'
   env.JAVA_HOME = '/usr'

   stage('Run-ut') {   
       echo 'Unstash the project source code ...'
       unstash 'SOURCE_CODE' 

       echo 'Run unit tests ...'
       sh "'${M2_HOME}/bin/mvn' clean test"
  }
}



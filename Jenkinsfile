
node('ubuntu') {
  stage 'Build'
    sh 'bash ./build.sh'

  stage 'Unit Tests'
    unitTests {}
}

node('webdriver'){
  stage 'Automated Tests'
    env.PATH = env.PATH.replaceAll('"', '')
    env.WORKSPACE = 'c:/jenkins/workspace/test_executor_${EXECUTOR_NUMBER}'
    checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'e3b68f9c-9a58-4c10-9f5b-b6f3754322fb', url: 'git@github.com:Wikia/selenium-tests.git']]])
    bat "gradlew.bat test -Dgroups=WamPage001 -DmockAds=true -Denv=prod -Dbrowser=CHROME -g %WORKSPACE% && exit %%ERRORLEVEL%%"
    archive '**/logs/, **/testng-results.xml, **/testng-failed.xml'
}

  stage 'Archive'
    archive '*.xml'
}

pipeline {
agent any
stages {
/**Insurance-Backend Pipeline Job Build and Test stages **/
stage('SCM Checkout') {
steps {
git url:'https://github.com/kavitharajoji/INGFavBank.git'
}
}
stage('Build') {
steps {
    sh'/opt/maven/bin/mvn clean package -Dmaven.test.skip=true'
}
}
stage('Build Analysis') {
          steps {
withSonarQubeEnv('sonar')  
                  {
    sh '/opt/maven/bin/mvn clean verify sonar:sonar'
                }
  }
         }
stage('Quality Gate') {
            steps {
              timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
           }
          }
stage('Deploy') {
steps {
    sh'/opt/maven/bin/mvn clean deploy -Dmaven.test.skip=true'
}
}
stage('Release') {
steps {
    sh'export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &'
}
}
}
}


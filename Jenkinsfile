node {
  def server = Artifactory.server 'docedson.jfrog.io'
  def myGradleContainer = docker.image('gradle:jdk8-alpine')
  myGradleContainer.pull()
  stage('prep') {
    checkout scm
  }
  stage('build') {
     myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
       sh 'cd complete && ./gradlew build'
     }
  }
  stage('test') {
     myGradleContainer.inside("-v ${env.HOME}/.gradle:/home/gradle/.gradle") {
       sh 'cd complete && ./gradlew test'
     }
  }
  stage('publish') {
    def uploadSpec = """{
      "files": [
        {
          "pattern": "complete/build/libs/gs-gradle-*.jar",
          "target": "example-gradle-dev-local/"
        }
     ]
    }"""
    server.upload(uploadSpec)
  }
}

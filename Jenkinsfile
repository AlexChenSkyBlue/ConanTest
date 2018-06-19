def artifactory_name = "art-c"
def artifactory_repo = "conan-local"
def repo_url = 'https://github.com/AlexChenSkyBlue/ConanTest.git'
def repo_branch = 'master'
node {
   def server
   def client
   def serverName
stage("Get project"){
    git branch: repo_branch, url: repo_url
}
stage("Configure Artifactory/Conan"){
    server = Artifactory.server artifactory_name
    client = Artifactory.newConanClient()
    serverName = client.remote.add server: server, repo: artifactory_repo
}
stage("Get dependencies and publish build info"){
    sh "mkdir -p build"
    dir ('build') {
      def b = client.run(command: "install .. -s os='Linux' -s compiler='gcc'")
      server.publishBuildInfo b
    }
}
    stage("Build/Test project"){
        dir ('build') {
          sh "cmake .. -G 'Unix Makefiles' -DCMAKE_BUILD_TYPE=Release && cmake --build ."
        }
    }
}

pipeline {
  agent {
    docker {
      image 'pendragon1902/dotnet-builder'
    }
  }
  environment {
    NEXUS_VERSION = "nexus3"
    NEXUS_PROTOCOL = "http"
    NEXUS_URL = "http://192.168.75.132:8081"
    NEXUS_REPOSITORY = "test_dotnet"
    NEXUS_API_KEY="57bdbf3a-85d3-31fc-968e-25714a2ff243"
  }
  stages {
    stage('clone repo'){
      steps{
        git branch: 'main', url: 'https://github.com/propropro1905/ConsoleSample.git'
      }
    }
    stage('build and scan sonar'){
      steps {
        sh 'dotnet-sonarscanner begin -k:"test_dotnet" -d:sonar.host.url=http://192.168.75.132:9000 -d:sonar.login=5bf81fa069a71f925b33640719d88f83488b0df8 '
        sh 'dotnet build'
        sh 'dotnet-sonarscanner end -d:sonar.login=5bf81fa069a71f925b33640719d88f83488b0df8'
      }
    }
  }
    stage('push nuget package nexus repo'){
      steps{
        sh 'dotnet package'
        sh 'nuget sources add -Source  {NEXUS_URL}/repository/{NEXUS_REPOSITORY}/ -Name test_dotnet -Username admin -Password abc251199'
        sh 'nuget setapikey {NEXUS_API_KEY} -source {NEXUS_URL}/repository/{NEXUS_REPOSITORY}/'
        sh 'dotnet nuget push ./App/bin/Debug/*.nupkg --source {NEXUS_URL}/repository/{NEXUS_REPOSITORY} --api-key {NEXUS_API_KEY}'
      }
    }
  }
}

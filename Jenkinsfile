// Edit your app's name below
def APP_NAME = 'sm-bridge-service'

// Edit your environment TAG names below
def TAG_NAMES = ['dev', 'test', 'prod']

// You shouldn't have to edit these if you're following the conventions
def BUILD_CONFIG = APP_NAME;
def IMAGESTREAM_NAME = APP_NAME

node {

  stage('build sm-bridge-service') {
    echo "Building: " + NGINX_BUILD_CONFIG
    openshiftBuild bldCfg: NGINX_BUILD_CONFIG, showBuildLogs: 'true'
  }

  stage('build ' + BUILD_CONFIG) {
    echo "Building: " + BUILD_CONFIG
    openshiftBuild bldCfg: BUILD_CONFIG, showBuildLogs: 'true'
    openshiftTag destStream: IMAGESTREAM_NAME, verbose: 'true', destTag: '$BUILD_ID', srcStream: IMAGESTREAM_NAME, srcTag: 'latest'
  }
  stage('deploy-' + TAG_NAMES[0]) {
    openshiftTag destStream: IMAGESTREAM_NAME, verbose: 'true', destTag: TAG_NAMES[0], srcStream: IMAGESTREAM_NAME, srcTag: '$BUILD_ID'
  }
}

node {
  stage('deploy-' + TAG_NAMES[1]) {
    input "Deploy to " + TAG_NAMES[1] + "?"
    openshiftTag destStream: IMAGESTREAM_NAME, verbose: 'true', destTag: TAG_NAMES[1], srcStream: IMAGESTREAM_NAME, srcTag: '$BUILD_ID'
  }
}

node {
  stage('deploy-'  + TAG_NAMES[2]) {
    input "Deploy to " + TAG_NAMES[2] + "?"
    openshiftTag destStream: IMAGESTREAM_NAME, verbose: 'true', destTag: TAG_NAMES[2], srcStream: IMAGESTREAM_NAME, srcTag: '$BUILD_ID'
  }
}

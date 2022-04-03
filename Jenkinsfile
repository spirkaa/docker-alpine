node {
  env.REGISTRY = 'git.devmem.ru'
  env.REGISTRY_CREDENTIALS_ID = 'gitea-user'
  env.IMAGE_OWNER = 'cr'
  env.IMAGE_NAME = 'alpine'
  env.IMAGE_TAG = '3.13-cron'
  env.DOCKERFILE = './3.13-cron/Dockerfile'
  env.LABEL_AUTHORS = 'Ilya Pavlov <piv@devmem.ru>'
  env.LABEL_TITLE = 'Alpine Linux'
  env.LABEL_DESCRIPTION = 'Alpine Linux with s6-overlay and cron'
  env.LABEL_CREATED = sh(script: "date '+%Y-%m-%dT%H:%M:%S%:z'", returnStdout: true).toString().trim()

  properties([
    buildDiscarder(
      logRotator(
        daysToKeepStr: '60',
        numToKeepStr: '10'
      )
    )
  ])

  stage('Checkout') {
    def scmVars = checkout scm
    env.GIT_COMMIT = scmVars.GIT_COMMIT.take(7)
    env.GIT_URL = scmVars.GIT_URL.toString().trim()
  }

  stage('Build') {
    // Build image
    docker.withRegistry("https://${env.REGISTRY}", "${env.REGISTRY_CREDENTIALS_ID}") {
      env.DOCKER_BUILDKIT = 1
      env.CACHE_FROM = "${env.REGISTRY}/${env.IMAGE_OWNER}/${env.IMAGE_NAME}:${env.IMAGE_TAG}"

      def myImage = docker.build(
        "${env.IMAGE_OWNER}/${env.IMAGE_NAME}:${env.IMAGE_TAG}",
        "--label \"org.opencontainers.image.created=${env.LABEL_CREATED}\" \
        --label \"org.opencontainers.image.authors=${env.LABEL_AUTHORS}\" \
        --label \"org.opencontainers.image.url=${env.GIT_URL}\" \
        --label \"org.opencontainers.image.source=${env.GIT_URL}\" \
        --label \"org.opencontainers.image.version=${env.IMAGE_TAG}\" \
        --label \"org.opencontainers.image.revision=${env.GIT_COMMIT}\" \
        --label \"org.opencontainers.image.title=${env.LABEL_TITLE}\" \
        --label \"org.opencontainers.image.description=${env.LABEL_DESCRIPTION}\" \
        --progress=plain \
        --cache-from ${env.CACHE_FROM} \
        -f ${env.DOCKERFILE} ."
      )
      myImage.push()
      // Untag and remove image by sha256 id
      sh "docker rmi -f \$(docker inspect -f '{{ .Id }}' ${myImage.id})"
    }
  }
}

node {
  def project = 'concise-hue-278020' 
  def appName = 'onlineshopping-service'
  def nameSpace='onlineshopping'
  def cluster='onlineshopping-cluster'
  def region='us-west1-b'
  def feSvcName = "PROJECT-${appName}"
  def imageTag = "gcr.io/${project}/${appName}:${env.BRANCH_NAME}.5"
  
  checkout scm
  
 // stage 'Build image'
  sh("docker build -t ${imageTag} .")

  stage 'Run node tests'
  //sh("docker run ${imageTag} node test")
  stage 'Skipping node tests'
  stage 'Push image to registry'

  // withEnv(['GCLOUD_PATH=/var/jenkins_home/google-cloud-sdk/bin']) {
  //               sh("gcloud docker -- push ${imageTag}")
  // }

  sh("gcloud docker -- push ${imageTag}")
  stage "Deploy Application"
  switch (env.BRANCH_NAME) {
  case "master":
  sh("kubectl get ns onlineshopping || kubectl create ns onlineshopping")
  sh("sed -i.bak 's#gcr.io/gcr-project/sample:1.0.0#${imageTag}#' ./k8s/${nameSpace}/*.yaml")
  sh("kubectl --namespace=${nameSpace} apply -f ./k8s/services/ --validate=false")
  sh("kubectl --namespace=${nameSpace} apply -f ./k8s/${nameSpace}/ --validate=false")
  }
  
}



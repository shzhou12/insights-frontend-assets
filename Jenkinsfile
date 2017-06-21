def wrapStep(String stepName, Closure step) {
  println "In wrapStep ${stepName}"
  try {
    step(stepName)
  } catch (e) {
    notify('FAILED', stepName)
    throw e
  }
}


node('insights-frontend-slave') {
  wrapStep('clone', { name -> stage(name) { checkout scm } })
  wrapStep('deploy_insights', { name -> stage(name) { sh 'rsync -arv -e "ssh -2" static sshacs@unprotected.upload.akamai.com:/114034/insights/' } })
  wrapStep('deploy_insightsbeta', { name -> stage(name) { sh 'rsync -arv -e "ssh -2" static sshacs@unprotected.upload.akamai.com:/114034/insightsbeta/' } })
}
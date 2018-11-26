def wrapStep(String stepName, Closure step) {
  println "In wrapStep ${stepName}"
  step(stepName)
}

node('python27') {
  stage('write_ssh_key') {
    writeFile file: "/tmp/akamai-ssh", text: "${env.AKAMAI_SSH_KEY}\n-----END RSA PRIVATE KEY-----"
    sh "chmod 600 /tmp/akamai-ssh"
  }
  if ('master' == env.BRANCH_NAME) {
    wrapStep('clone', { name -> stage(name) { checkout scm } })
    wrapStep('deploy_to_frontend_legacy', { name -> stage(name) { sh "rsync -arv -e \"ssh -i /tmp/akamai-ssh -o StrictHostKeyChecking=no\" * sshacs@unprotected.upload.akamai.com:/114034/insights/static/frontend-legacy/"} })
    wrapStep('deploy_insights', { name -> stage(name) { sh "rsync -arv -e \"ssh -i /tmp/akamai-ssh -o StrictHostKeyChecking=no\" * sshacs@unprotected.upload.akamai.com:/114034/insights/static/"} })
    wrapStep('deploy_insightsbeta', { name -> stage(name) { sh "rsync -arv -e \"ssh -i /tmp/akamai-ssh -o StrictHostKeyChecking=no\" * sshacs@unprotected.upload.akamai.com:/114034/insightsbeta/static"} })
    wrapStep('deploy_insightsalpha', { name -> stage(name) { sh "rsync -arv -e \"ssh -i /tmp/akamai-ssh -o StrictHostKeyChecking=no\" * sshacs@unprotected.upload.akamai.com:/114034/insightsalpha/static/"} })
    wrapStep('deploy_legacy_api', { name -> stage(name) { sh "rsync -arv -e \"ssh -i /tmp/akamai-ssh -o StrictHostKeyChecking=no\" * sshacs@unprotected.upload.akamai.com:/114034/r/insights/v1/static/"} })
  }
}

node {
  catchError {
    def nexusRepo = 'https://www.silverpeas.org/nexus/content/repositories/snapshots/'
    docker.image('silverpeas/silverbuild')
        .inside('-u root -v $HOME/.m2/settings.xml:/root/.m2/settings.xml -v $HOME/.m2/settings-security.xml:/root/.m2/settings-security.xml -v $HOME/.gitconfig:/root/.gitconfig -v $HOME/.ssh:/root/.ssh -v $HOME/.gnupg:/root/.gnupg') {
      stage('Preparation') {
        checkout scm
      }
      stage('Build and deployment') {
        sh "mvn clean deploy -DaltDeploymentRepository=silverpeas::default::${nexusRepo} -Djava.awt.headless=true"
      }
    }
  }
  step([$class                  : 'Mailer',
        notifyEveryUnstableBuild: true,
        recipients              : "miguel.moquillon@silverpeas.org, yohann.chastagnier@silverpeas.org, nicolas.eysseric@silverpeas.org",
        sendToIndividuals       : true])
}
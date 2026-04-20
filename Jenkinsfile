node {
    def Service     = params.Service
    def Serveur     = params.Serveur
    def ImageDocker = params.ImageDocker

    stage('Deploy - Clone') {
        git credentialsId: 'github-creds',
            url: 'https://github.com/azizm-git/jmeter-app.git',
            branch: 'main'
    }

    stage('Deploy - Run') {
        ansiblePlaybook(
            colorized: true,
            become: true,
            playbook: 'playbook.yml',
            inventory: "${Serveur},",
            extras: "--extra-vars 'image=${ImageDocker}'"
        )
    }

    stage('Test') {
        sh "sed -i 's/192.168.170.131/${Serveur}/g' plan-jenkins.jmx"
        sh '/opt/jmeter/bin/jmeter -n -t plan-jenkins.jmx -l results.jtl'
        sh 'cat results.jtl'
        perfReport 'results.jtl'
    }
}

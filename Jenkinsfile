pipeline
{
    agent none

    stages
    {
        stage('cloning git repo')
        {
            agent
            {
                label 'ubuntu-Appserver-3120'
            }
            steps
            {
                checkout scm
            }

        }

        stage('SCA-SAST-SNYK-TEST')
        {
            agent
            {
                label 'ubuntu-Appserver-3120'
            }
            steps
            {
                snykSecurity 
                {
                    snykInstallation: 'snykgame',
                    snykTokenId: 'snykid',
                    severity: 'critical'
                }
            }

        }

        stage('Build and Tag')
        {
            agent
            {
                label 'ubuntu-Appserver-3120'
            }
            steps
            {
                script
                {
                    def app = docker.build("xamq/snyk")
                    app.tag("latest")
                }
            }

        }

        stage('Push to docker')
        {
            agent
            {
                label 'ubuntu-Appserver-3120'
            }
            steps
            {
                script
                {
                    docker.withRegistry("https://registry.hub.docker.com", "maxq")
                    {
                        def app = docker.image("xamq/snyk")
                    }
                }
            }

        }

        stage('Deploy')
        {
            agent
            {
                label 'ubuntu-Appserver-3120'
            }
            steps
            {
                sh "docker-compose down"
                sh "docker-compose up -d"
            }

        }
    }
}

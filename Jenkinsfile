pipeline {
    agent any

    stages {
        stage('compile') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/learndevops-083/samplemaven.git']]])
                sh '/opt/maven/bin/mvn compile '
            }
        }
        stage('code-review') {
            steps {
                sh '/opt/maven/bin/mvn -P metrics pmd:pmd  '
            }
            post {
                success{
                    recordIssues(tools: [pmdParser(pattern: '**/pmd.xml')])
                }
            }
        }
        stage('package') {
            steps {
                sh '/opt/maven/bin/mvn package'
            }
        }
        
        stage('publish to jfrog') {
            steps {
                rtUpload (
                    serverId: 'jfrog-dev',
                          spec: '''{
                           "files": [
                             {
                               "pattern": "target/kitchensink.war",
                               "target": "example-repo-local/"
                            }
                        ]
                    }'''
                 )
            }
        }
        
    }
}

pipeline {
    agent any
    tools {
         git 'Default'
         jdk 'JAVA_HOME'
         maven 'M2_HOME'
        }
    stages {
        
        stage('Code Checkout') {
            steps {
                echo "code checkout"
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/PranabandhuSahu/SaiJavaCode.git']]])
            }
        }

        stage('Build Code') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Deploy - Production') {
            steps {
               echo "deploy to server"
              sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible-server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /home/ansadmin/deploy_to_tomcat.yml;', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: 'webapp/target', sourceFiles: 'webapp/target/*.war')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}

pipeline {
    agent any
	stages {
	    stage('Build') {
			steps {
				echo 'Running Build Automation',
				archiveArtifacts artifacts: 'dist/trainschedule.zip'
			}
		}
		stage('DeployToStaging') {
			when {
				branch 'master'
			}
			steps {
				withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
					sshPublisher(
						publishers: [
							sshPublisherDesc(
								configName: 'staging',
								sshCredentials: [,
									username: "$USERNAME",
									Password: "$USERPASS"
						],
						transfers: [
							sshTransfer(
								sourceFiles: 'dist/trainschedule.zip',
								removePrefix: 'dist/',
								remoteDirectory: '/tmp',
								execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainschedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
							)
						]
					)
				}
			}
		}
	}
}
				

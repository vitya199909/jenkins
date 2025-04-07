pipeline {
    agent { label 'vm2' }

    stages {
        stage('Install Apache') {
            steps {
                echo "Installing Apache server on Ubuntu..."
                withCredentials([usernamePassword(credentialsId: 'jenkins-sudo', usernameVariable: 'USERNAME', passwordVariable: 'SUDO_PASS')]) {
                    sh '''
                        echo "$SUDO_PASS" | sudo -S apt update
                        echo "$SUDO_PASS" | sudo -S apt install -y apache2
                        echo "$SUDO_PASS" | sudo -S systemctl enable apache2
                        echo "$SUDO_PASS" | sudo -S systemctl start apache2
                    '''
                }
            }
        }

        stage('Verify Apache Status') {
            steps {
                echo "Verifying Apache service status..."
                withCredentials([usernamePassword(credentialsId: 'jenkins-sudo', usernameVariable: 'USERNAME', passwordVariable: 'SUDO_PASS')]) {
                    sh '''
                        echo "$SUDO_PASS" | sudo -S systemctl status apache2 || true
                    '''
                }
            }
        }

        stage('Check Apache Logs for Errors') {
            steps {
                echo "Checking Apache logs for 4xx and 5xx errors..."
                withCredentials([usernamePassword(credentialsId: 'jenkins-sudo', usernameVariable: 'USERNAME', passwordVariable: 'SUDO_PASS')]) {
                    sh '''
                        echo "$SUDO_PASS" | sudo -S tail -n 100 /var/log/apache2/error.log
                        echo "$SUDO_PASS" | sudo -S grep -E " 4[0-9]{2} | 5[0-9]{2}" /var/log/apache2/error.log || echo "No 4xx or 5xx errors found"
                    '''
                }
            }
        }
    }
}

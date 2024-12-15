pipeline {
    agent any
    environment {
        APACHE_LOG_PATH = '/var/log/apache2/access.log' // Path to Apache log file
    }
    stages {
        stage('Verify Log File Exists') {
            steps {
                script {
                    echo 'Checking if the log file exists...'
                    sh '''
                        if [ ! -f ${APACHE_LOG_PATH} ]; then
                            echo "Log file not found: ${APACHE_LOG_PATH}"
                            exit 1
                        fi
                    '''
                }
            }
        }
        stage('Check for 4xx and 5xx Errors') {
            steps {
                script {
                    echo 'Scanning for 4xx and 5xx errors in the log file...'
                    sh '''
                        # Extract 4xx and 5xx errors from the Apache log file
                        grep -E '\\s4[0-9]{2}\\s|\\s5[0-9]{2}\\s' ${APACHE_LOG_PATH} > /home/jenkins/apache_error_lines.log || true

                        if [ -s /home/jenkins/apache_error_lines.log ]; then
                            echo "Errors found in the log file:"
                            cat /home/jenkins/apache_error_lines.log
                        else
                            echo "No 4xx or 5xx errors found in the log file."
                        fi
                    '''
                }
            }
        }
    }
}

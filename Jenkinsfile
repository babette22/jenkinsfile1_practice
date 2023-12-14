pipeline {
    agent any

    parameters {
        choice(name: 'ACTION', choices: ['Add User', 'Delete User', 'Lock User', 'Unlock User'], description: 'Select the action to perform')
        string(name: 'USERNAME', defaultValue: '', description: 'Username')
        string(name: 'PASSWORD', defaultValue: '', description: 'Password')
        string(name: 'FIRST_NAME', defaultValue: '', description: 'First Name')
        string(name: 'LAST_NAME', defaultValue: '', description: 'Last Name')
        string(name: 'EMAIL', defaultValue: '', description: 'Email')
    }

    stages {
        stage('Example') {
            steps {
                script {
                    echo "Selected Action: ${params.ACTION}"
                    echo "Username: ${params.USERNAME}"
                    echo "Password: ${params.PASSWORD}"
                    echo "First Name: ${params.FIRST_NAME}"
                    echo "Last Name: ${params.LAST_NAME}"
                    echo "Email: ${params.EMAIL}"

                    def passwordCommand = sh(script: "echo -n ${params.PASSWORD} | openssl passwd -1 -stdin", returnStdout: true).trim()

                    // Perform actions based on the selected choice
                    switch(params.ACTION) {
                        case 'Add User':
                            sh "echo ${params.PASSWORD} | sudo -S tee -a /dev/null | sudo -S useradd -m -p ${passwordCommand} -c 'Full Name: ${params.FIRST_NAME} ${params.LAST_NAME}' -e ${params.EMAIL} ${params.USERNAME}"
                            echo "User ${params.USERNAME} added"
                            break
                        case 'Delete User':
                            sh "echo ${params.PASSWORD} | sudo -S tee -a /dev/null | sudo -S userdel -r ${params.USERNAME}"
                            echo "User ${params.USERNAME} deleted"
                            break
                        case 'Lock User':
                            sh "echo ${params.PASSWORD} | sudo -S tee -a /dev/null | sudo -S usermod -L ${params.USERNAME}"
                            echo "User ${params.USERNAME} locked"
                            break
                        case 'Unlock User':
                            sh "echo ${params.PASSWORD} | sudo -S tee -a /dev/null | sudo -S usermod -U ${params.USERNAME}"
                            echo "User ${params.USERNAME} unlocked"
                            break
                        default:
                            error "Invalid action selected"
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed!'
        }
    }
}

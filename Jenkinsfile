pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    parameters{
        string(name: 'env',defaultValue: 'prd',description: 'uat,prd')
        string(name: 'update',defaultValue: 'no',description: 'yes,no')
        credentials(credentialType: 'org.jenkinsci.plugins.plaincredentials.impl.FileCredentialsImpl',
                    defaultValue: 'ansible-vault-2', description: '', name: 'secret_id', required: true)
    }
    stages {
        stage ('Deploy') {
            steps{
            withCredentials([file(credentialsId: params.secret_id, variable: 'secrets')]){
                sh """
                cd ${WORKSPACE}/ansible;
                ansible-playbook -i host test3.yml -v --vault-password-file "${secrets}" -e "@secrets/debug.yml" \
                                                -e "envv=${params.env}" \
                                                -e "src_path=${WORKSPACE}" \
                                                -e "update=${params.update}"
                """
            }
            }
            post {
                success {
                    echo "Deploy FINISH"
                }
            }   
        }
    }
}

# Ansible Collection - test1.httpd

node("master"){
       stage ('SCM Checkout') { 
         
         checkout([$class: 'GitSCM', branches: [[name: "master"]], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, parentCredentials: false, recursiveSubmodules: false, reference: '', trackingSubmodules: false]], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'f345e025-edee-471d-9b95-cc26257d34ec', url: "https://gitlab.com/ssouteh/${COLLECTION}.git"]]])
                       }

        stage ('Ansible Galaxy Build') {
        sh 'rm -rf *.tar.gz'
        sh 'export PATH=$PATH:/usr/local/bin ; ansible-galaxy collection build'    
          }
        stage ('Ansible Galaxy Push') {
        sh 'export PATH=$PATH:/usr/local/bin ; ansible-galaxy collection publish $(ls *.tar.gz) -s http://163.172.145.86:8000 --api-key ${TOKEN}' 
          }
    
} 

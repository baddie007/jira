properties([pipelineTriggers([githubPush()])])

pipeline {
       agent any
  tools {
    maven 'Maven'
  }
  environment {
   registry = "arpit74/mytest"
   registryCredential = "38c46c6-0260-4af4-adee-fe5d19c229ec"
  }
  stages {
         
         stage('Checkout SCM') {
            steps {
                checkout([
                 $class: 'GitSCM',
                 branches: [[name: 'master']],
                 userRemoteConfigs: [[
                    url: 'git@github.com:baddie007/jira.git',
                    credentialsId: 'github',
                 ]]
                ])
            }
        }
         
         
         
    stage('Initialize'){
      steps{
        echo "We are doing some test for integration"
        echo "PATH = ${PATH}"
        }
    }
    stage('Build'){
           steps
           {
        sh "mvn clean install"
    }     
    }
        
  }
post {
       always {
            echo 'I will always say Hello again!'
     create_newjira_issue()
       }
    }

}
void create_newjira_issue() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'HAC'],
            summary: 'Maven Build',
            description: 'Facing some issue in building Maven Code',
            issuetype: [name:'Task']]]


    response = jiraNewIssue issue: NewJiraIssue ,site: 'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}




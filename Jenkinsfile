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
                    url: 'https://github.com/baddie007/jira.git',
                    credentialsId: 'github',
                 ]]
                ])
            }
        }
         
         
         
    stage('Initialize'){
      steps{
        echo "We're Initialising Build Now!!!"
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
       success{
           echo "Great, Build was successful!!"
           build_success()
       }

       failure{
           echo "Oops, Build was failed!!"
           newjira_issue()
       }
    }

}
void newjira_issue() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'HAC'],
            summary: 'Build Failure',
            description: 'Facing some issue in building Maven Code',
            issuetype: [name:'Task']]]


    response = jiraNewIssue issue: NewJiraIssue ,site: 'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}

void build_success() {
    node {
      stage('JIRA') {
        def NewJiraIssue = [fields: [project: [key: 'HAC'],
            summary: 'Successful Build',
            description: 'Build was successful',
            issuetype: [name:'Task']]]


    response = jiraNewIssue issue: NewJiraIssue ,site: 'JIRA'

    echo response.successful.toString()
    echo response.data.toString()
    }
  }
}

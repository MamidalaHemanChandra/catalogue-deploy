@Library('jenkins-shared-library') _

properties([
    parameters([
        string(name: 'appVersion',defaultValue: ''),
        string(name: 'deploy_to',defaultValue: '')     
    ])
])

def configMap = [
    project  : "roboshop",
    component: "catalogue",
    appVersion: (params.appVersion),
    deploy_to: (params.deploy_to)
]

deploy(configMap)
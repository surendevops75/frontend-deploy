// --------------------------------------------------
// Load Jenkins Shared Library
// --------------------------------------------------

@Library('jenkins-shared-library') _

// --------------------------------------------------
// Define Build Parameters
// --------------------------------------------------

properties([

  parameters([

    // Application version to deploy
    string(
      name: 'appVersion',
      description: 'Enter Application version'
    ),

    // Deployment environment
    choice(
      name: 'deploy_to',
      choices: ['dev', 'qa', 'prod'],
      description: 'Target environment'
    )

  ])
])

// --------------------------------------------------
// Build Configuration Map
// --------------------------------------------------

def configMap = [

  // Project Name
  project : "roboshop",

  // Application Component
  component : "frontend",

  // Environment Selection
  deploy_to : (params.deploy_to ?: 'dev'),

  // Image Version
  appVersion : (params.appVersion)
]

// --------------------------------------------------
// Print Deployment Information
// --------------------------------------------------

echo "Going to execute Jenkins shared library"

echo "ConfigMap: ${configMap}"

// --------------------------------------------------
// Execute Deployment Function
// --------------------------------------------------

EKSDeploy(configMap)
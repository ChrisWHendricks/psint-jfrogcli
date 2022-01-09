# Github Action for Setup jfrog cli for Artifactory
Sets up jFrog CLI for artifactory so it can be used in Github action workflows.  
This action will download the jfrog CLI and will configure a jfrog Server with provided url, username, and password

## Usage
### Inputs
- url - *Required* Url to your artifactory instance. Defaults to ${{ env.ARTIFACTORY_URL }}
- username - *Required* Username for authenticating to your artifactory instance. Defaults to ${{ env.ARTIFACTORY_USERNAME }}
- password - *Required* Password or API Key for artifactory user Defaults to ${{ env.ARTIFACTORY_PASSWORD }}

### Environment Variables
Instead of providing input parameters the necessary information can be set as environment variables.
| Variable Name | Description |
| ------------- | ------------|
| ARTIFACTORY_URL| The main artifactory url, usually ends in artifactory i.e. https://domain.jfrog.io/artifactory |
| ARTIFACTORY_USERNAME | Username for authenticating to Artifactory |
| ARTIFACTORY_PASSWORD | Password or Generated API Key for the value provided in the ARTIFACTORY_USERNAME environment variable |

### Example
```
name: CI-JOB
on: 
  workflow_dispatch
  push
jobs: Build
env:     
    ARTIFACTORY_URL: "https://tylertech.jfrog.io/artifactory"
    # Username and password should always be a secret
    ARTIFACTORY_USERNAME: "${{ secrets.ARTIFACTORY_USERNAME }}"   
    ARTIFACTORY_PASSWORD: "${{ secrets.ARTIFACTORY_PASSWORD }}"
    
steps:    
  - uses: actions/checkout@v2
  
  # inputs provided, action will use input values instead of environment variables
  - id: setupJfrog
        uses: chriswhendricks/psint-jfrogcli/actions/setup@feat/setup-action
       with:
          url: https://tylertech.jfrog.io/artifactory
          userName: ${{ secrets.ARTIFACTORY_USERNAME }}
          password: ${{ secrets.ARTIFACTORY_PASSWORD }}
  
  # No inputs provided, action will read values from environment variables      
   - id: setupJfrog
        uses: chriswhendricks/psint-jfrogcli/actions/setup@feat/setup-action
        
 # execute ping to verify authentication was successfull       
 - id: pingJfrog
        run: jfrog rt ping
```

## Recommended Secrets
It is highly recommended to store Artifactory Username and Password in a repositorty secret
- ARTIFACTORY_USERNAME
- ARTIFACTORY_PASSWORD

You may also want to store the artifactory url in a secret, but this is not as critical as the username and password

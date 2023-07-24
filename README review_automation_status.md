# Itential Automation Status 
Github Action to monitor Itential Automation Platform(IAP) automations status and return output variables

## Table of Contents 
  - [Prerequisites] (#prerequisite)
  - [Supported IAP Versions](#supported-iap-versions)
  - [Getting Started](#getting-started)
  - [Configurations](#configurations)
    - [Required Input Parameters](#required-input-parameters)
    - [Optional Input Parameters](#optional-input-parameters)
    - [Output](#output)
  - [Example Usage](#example-usage)

## Prerequisites  
An Itential account is required to get credentials needed to configure the Github Actions.
In order to utilize this action, you would need to have an active `Itential Automation Platform` (IAP).\
If you are an existing customer, please contact your Itential account team for additional details.
For new customers interested in an Itential trial, please click [here](https://www.itential.com/get-started/) to request one.

## Supported IAP Versions #Review
* 2023.1
* 2022.1
* 2021.2
* 2021.1

## Getting Started #Review
1. Search for the Action on Github Marketplace.
2. Select the "Use the Latest Version" option on the top right of the screen 
3. Click the clipboard icon to copy the provided data. 
4. Navigate to the 'github/workflows' file in the target repository (where you intend on using the action. )
5. Paste the copied data in the correlating fields. 
6. Configure the required inputs and optional inputs. (See note)
7.  Save it as main.yml file.   #Review

>**_Note:_** Users may manually enter required input parameters or use Github Secrets if they want to hide certain parameters. If you choose to use Github Secrets, please reference the instructions provided below. 

1. Select the settings tab on your target repository 
2. Select the secrets and variables tab under security options 
3. Click the "new repository secret"option on the top right of the screen 
4. Enter the required fields 
For YOUR_SECRET_NAME enter a required input. 
For SECRET enter your desired variable. 
6. Click "Add Secret"

See [action.yml](action.yml) for [metadata](https://docs.github.com/en/actions/creating-actions/metadata-syntax-for-github-actions) that defines the inputs, outputs, and runs configurations for this action._

 ### Required Input Parameters
The following table defines the required parameters to monitor IAP Automation Status using a Github workflow. Input data is provided through Github Actions secrets. For more information about github action secrets, see [Github Secrets](https://docs.github.com/en/rest/actions/secrets?apiVersion=2022-11-28)
 
| Parameter | Description |
| --------- | ----------- |
| iap_instance | URL to the IAP Instance |
| iap_token | To authenticate API requests to the instance |
| automation_id| Automation ID to check status of the automation |

### Optional Input Parameters
The following table defines parameters considered optional. 

| Parameter | Description | Default |
| --------- | ----------- | ------- |
| time_interval | Time interval to check automation status (in seconds) | 15  |
| no_of_attempts | No of attempts to check automation status | 10 |

### Output
The following table defines parameters that are returned. 
| Parameter | Description |
| --------- | ----------- |
| results | Automation Status output variables | #Review


## Example Usage 

The example below displays how to configure a workflow that runs when issues or pull requests are opened or labeled in your repository. This workflow runs when new pull request is opened as defined in the `on` variable of the workflow.

This action is defined in  `job1` of the workflow by the step id `step1`. The output of this step is defined as `output1` of the workflow and is extracted using the output variable of this action i.e. `results` as defined in the output.

You have the option to configure any filters you may want to add, such as only triggering workflow with a specific branch. 


_For more information about workflows, see [Using workflows](https://docs.github.com/en/actions/using-workflows)._

```yaml
# This is a basic workflow to help you get started with Actions
name: Get Automation Status
# Controls when the workflow will run
on:
  pull_request:
    types: [opened]

jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      output1: ${{ steps.step1.outputs.results }}
    name: Automation Status
    steps:
      # To use this repository's private action,
      # you must check out the repository
      - name: Checkout
        uses: actions/checkout@v3
      - name: Hello world action step
        id: step1
        uses: itential/itential-automation-status@version
        with:
          iap_token: ${{secrets.IAP_TOKEN}}
          iap_instance: ${{secrets.IAP_INSTANCE}}
          automation_id: ${{secrets.AUTOMATION_ID}}
          no_of_attempts: 10
          time_interval: 15
      - name: Get output
        run: echo "${{steps.step1.outputs.results}}"
```
# Azure_DotNet_VSTest
Use this task to run unit and functional tests (Selenium, Appium, Coded UI test, etc.) using the Visual Studio Test (VSTest) runner. You can run test frameworks that have a Visual Studio test adapter. 

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |


These parameters provide multiple use case options for the dotnet templates pipeline, enable/disable flags for the utilization of different templates as per the requirements.


## Use Cases

You can directly call a particular template as per the requirement. for example: 

  ```yaml
  # azure-pipeline.yml
  resources:
  repositories:
    - repository: Template
      type: github
      name: your_username/Azure_DotNet_VSTest
      ref: <respective branch name>
      endpoint: 'githubServiceConnectioNname'

  steps:
  # passing the parameters
  - template: dotNetUnitTest.yml
    parameters:
      dotNetTestProjectFilePath: '${{ parameters.dotNetTestProjectFilePath }}' 
      dotNetTestArguements: '${{ parameters.dotNetTestArguements }}'
      publishTestResults: '${{ parameters.publishTestResults }}'
      testRunTitle: '${{ parameters.testRunTitle }}'
      workingDirectory: '${{ parameters.workingDirectory }}'                 

  ``` 
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

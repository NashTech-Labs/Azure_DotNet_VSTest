# Azure_DotNet_VSTest
Use this task to run unit and functional tests (Selenium, Appium, Coded UI test, etc.) using the Visual Studio Test (VSTest) runner. You can run test frameworks that have a Visual Studio test adapter. 

## Pipeline Requirements

The dotNet Module pipeline requires the following parameters to be defined:

Parameters:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | :-------------: | :-------------: | :-------------: | :-------------: | ------------- |
| testRunner | Test result format | String | JUnit | JUnit, NUnit, VSTest, XUnit, CTest |  Required | Specifies the format of the results files you want to publish. The following formats are supported: CTest, JUnit, NUnit 2, NUnit 3, Visual Studio Test (TRX) and xUnit 2 |
| testResultsFiles | Test results files | String | '**/*.trx' |  |  Required | Specifies one or more test results files |
| testSelector | Select tests using | String | testAssemblies | testAssemblies (Test assemblies), testPlan (Test plan), testRun (Test run) | Required | Test assembly: Specifies one or more test assemblies that contain your tests. You can optionally specify a filter criteria to select only specific tests.Test plan: Runs tests from your test plan that have an automated test method associated with it. Test run: Use this option when you are setting up an environment to run tests from test plans. This option should not be used when running tests in a continuous integration/continuous deployment (CI/CD) pipeline. |
| testAssemblyVer2 | Test files | String | **\bin\**\*test.dll\n**\bin\**\*tests.dll |  | Required | Required when testSelector = testAssemblies. Runs tests from the specified files. Ordered tests and webtests can be run by specifying the .orderedtest and .webtest files respectivel |
| searchFolder | Search folder | String | $(System.DefaultWorkingDirectory) |  | Required | Specifies the folder to search for the test assemblies |
| vstestLocationMethod | Select test platform using | String | version | version, location (Specific location) | Optional | Specifies which test platform to use |
| vsTestVersion | Test platform version | String | latest | latest, 17.0 (Visual Studio 2022), 16.0 (Visual Studio 2019), 15.0 (Visual Studio 2017), 14.0 (Visual Studio 2015), toolsInstaller (Installed by Tools Installer) | Optional | Use when vstestLocationMethod = version. Specifies the version of Visual Studio Test to use. To run tests without needing Visual Studio on the agent, use the Installed by tools installer option  |
| runOnlyImpactedTests | Run only impacted tests | Boolean | false | true, false | Optional | Use when testSelector = testAssemblies. Automatically specifies and runs the tests needed to validate the code change |
| uiTests | Test mix contains UI tests | Boolean | false | true, false | Optional | To run UI tests, ensure that the agent is set to run in interactive mode with Autologon enabled. Setting up an agent to run interactively must be done before queueing the build/release |
| failOnMinTestsNotRun | Fail the task if a minimum number of tests are not run | Boolean | false | true, false | Optional | Fails the task if a minimum number of tests are not run. This may be useful if any changes to task inputs or underlying test adapter dependencies lead to only a subset of the desired tests to be found |
| diagnosticsEnabled | Collect advanced diagnostics in case of catastrophic failures | Boolean | false | true, false | Optional | Collects diagnostic data to troubleshoot catastrophic failures, such as a test crash. When this option is checked, a sequence XML file is generated and attached to the test run |
| rerunFailedTests | Rerun failed tests | Boolean | false | true, false | Optional | Reruns any failed tests until they pass or until the maximum number of attempts is reached |
| publishRunAttachments | Upload test attachments | Boolean | true | true, false | Optional | Opts in or out of publishing run level attachments |
| runSettingsFile | Settings file | String |  |  | Optional | Specifies the path to a runsettings or testsettings file to use with the tests. For Visual Studio 15.7 and higher, use runsettings for all test types |
| overrideTestrunParameters | Override test run parameters | String |  |  | Optional | Overrides the parameters defined in the TestRunParameters section of a runsettings file or the Properties section of a testsettings file |
| pathtoCustomTestAdapters | Path to custom test adapters | String |  |  | Optional | Specifies the directory path to custom test adapters. Adapters residing in the same folder as the test assemblies are automatically discovered |

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
    displayName: Execute Unit Tests using VSTest
      testSelector: '${{ parameters.testSelector }}'
      testAssemblyVer2: '${{ parameters.testAssemblyVer2 }}'
      searchFolder: '${{ parameters.searchFolder }}'
      testFiltercriteria: '${{ parameters.testFiltercriteria }}'
      ${{ if parameters.resultsFolder }}:
        resultsFolder: '${{ parameters.resultsFolder }}'
      runInParallel: '${{ parameters.runInParallel }}'
      runTestsInIsolation: '${{ parameters.runTestsInIsolation }}'
      codeCoverageEnabled: '${{ parameters.codeCoverageEnabled }}'
      otherConsoleOptions: '${{ parameters.otherConsoleOptions }}'
      vstestLocationMethod: '${{ parameters.vstestLocationMethod }}'
      ${{ if eq( parameters.vstestLocationMethod, 'version') }}:
        vsTestVersion: '${{ parameters.vsTestVersion }}'
      ${{ if eq( parameters.vstestLocationMethod, 'location') }}:
        vstestLocation: '${{ parameters.vstestLocation }}'        
      runOnlyImpactedTests: '${{ parameters.runOnlyImpactedTests }}'
      uiTests: '${{ parameters.uiTests }}'
      runSettingsFile: '${{ parameters.runSettingsFile }}'
      overrideTestrunParameters: '${{ parameters.overrideTestrunParameters }}'
      pathtoCustomTestAdapters: '${{ parameters.pathtoCustomTestAdapters }}'
      failOnMinTestsNotRun: '${{ parameters.failOnMinTestsNotRun }}'
      diagnosticsEnabled: '${{ parameters.diagnosticsEnabled }}'
      rerunFailedTests: '${{ parameters.rerunFailedTests }}'
      publishRunAttachments: '${{ parameters.publishRunAttachments }}'                

  ``` 
  
Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.

# sam-app4

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

- hello_world - Code for the application's Lambda function.
- events - Invocation events that you can use to invoke the function.
- tests - Unit tests for the application code. 
- template.yaml - A template that defines the application's AWS resources.

The application uses several AWS resources, including Lambda functions and an API Gateway API. These resources are defined in the `template.yaml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.

If you prefer to use an integrated development environment (IDE) to build and test your application, you can use the AWS Toolkit.  
The AWS Toolkit is an open source plug-in for popular IDEs that uses the SAM CLI to build and deploy serverless applications on AWS. The AWS Toolkit also adds a simplified step-through debugging experience for Lambda function code. See the following links to get started.

* [CLion](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [GoLand](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [IntelliJ](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [WebStorm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [Rider](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [PhpStorm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [PyCharm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [RubyMine](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [DataGrip](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [VS Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)
* [Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/welcome.html)

## Deploy the sample application

The Serverless Application Model Command Line Interface (SAM CLI) is an extension of the AWS CLI that adds functionality for building and testing Lambda applications. It uses Docker to run your functions in an Amazon Linux environment that matches Lambda. It can also emulate your application's build environment and API.

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and deploy your application for the first time, run the following in your shell:

```bash
sam build --use-container
sam deploy --guided
```

The first command will build the source of your application. The second command will package and deploy your application to AWS, with a series of prompts:

* **Stack Name**: The name of the stack to deploy to CloudFormation. This should be unique to your account and region, and a good starting point would be something matching your project name.
* **AWS Region**: The AWS region you want to deploy your app to.
* **Confirm changes before deploy**: If set to yes, any change sets will be shown to you before execution for manual review. If set to no, the AWS SAM CLI will automatically deploy application changes.
* **Allow SAM CLI IAM role creation**: Many AWS SAM templates, including this example, create AWS IAM roles required for the AWS Lambda function(s) included to access AWS services. By default, these are scoped down to minimum required permissions. To deploy an AWS CloudFormation stack which creates or modifies IAM roles, the `CAPABILITY_IAM` value for `capabilities` must be provided. If permission isn't provided through this prompt, to deploy this example you must explicitly pass `--capabilities CAPABILITY_IAM` to the `sam deploy` command.
* **Save arguments to samconfig.toml**: If set to yes, your choices will be saved to a configuration file inside the project, so that in the future you can just re-run `sam deploy` without parameters to deploy changes to your application.

You can find your API Gateway Endpoint URL in the output values displayed after deployment.

## Use the SAM CLI to build and test locally

Build your application with the `sam build --use-container` command.

```bash
sam-app4$ sam build --use-container
```

The SAM CLI installs dependencies defined in `hello_world/requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

Test a single function by invoking it directly with a test event. An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

Run functions locally and invoke them with the `sam local invoke` command.

```bash
sam-app4$ sam local invoke HelloWorldFunction --event events/event.json
```

The SAM CLI can also emulate your application's API. Use the `sam local start-api` to run the API locally on port 3000.

```bash
sam-app4$ sam local start-api
sam-app4$ curl http://localhost:3000/
```

The SAM CLI reads the application template to determine the API's routes and the functions that they invoke. The `Events` property on each function's definition includes the route and method for each path.

```yaml
      Events:
        HelloWorld:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

## Add a resource to your application
The application template uses AWS Serverless Application Model (AWS SAM) to define application resources. AWS SAM is an extension of AWS CloudFormation with a simpler syntax for configuring common serverless application resources such as functions, triggers, and APIs. For resources not included in [the SAM specification](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md), you can use standard [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) resource types.

## Fetch, tail, and filter Lambda function logs

To simplify troubleshooting, SAM CLI has a command called `sam logs`. `sam logs` lets you fetch logs generated by your deployed Lambda function from the command line. In addition to printing the logs on the terminal, this command has several nifty features to help you quickly find the bug.

`NOTE`: This command works for all AWS Lambda functions; not just the ones you deploy using SAM.

```bash
sam-app4$ sam logs -n HelloWorldFunction --stack-name "sam-app4" --tail
```

You can find more information and examples about filtering Lambda function logs in the [SAM CLI Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-logging.html).

## Tests

Tests are defined in the `tests` folder in this project. Use PIP to install the test dependencies and run tests.

```bash
sam-app4$ pip install -r tests/requirements.txt --user
# unit test
sam-app4$ python -m pytest tests/unit -v
# integration test, requiring deploying the stack first.
# Create the env variable AWS_SAM_STACK_NAME with the name of the stack we are testing
sam-app4$ AWS_SAM_STACK_NAME="sam-app4" python -m pytest tests/integration -v
```

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
sam delete --stack-name "sam-app4"
```

## Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

Next, you can use AWS Serverless Application Repository to deploy ready to use Apps that go beyond hello world samples and learn how authors developed their applications: [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/)

## Sam init commands

```bash
@take-takashi ➜ /workspaces/try-sam (main) $ sam init

You can preselect a particular runtime or package type when using the `sam init` experience.
Call `sam init --help` to learn more.

Which template source would you like to use?
        1 - AWS Quick Start Templates
        2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
        1 - Hello World Example
        2 - Data processing
        3 - Hello World Example with Powertools for AWS Lambda
        4 - Multi-step workflow
        5 - Scheduled task
        6 - Standalone function
        7 - Serverless API
        8 - Infrastructure event management
        9 - Lambda Response Streaming
        10 - Serverless Connector Hello World Example
        11 - Multi-step workflow with Connectors
        12 - GraphQLApi Hello World Example
        13 - Full Stack
        14 - Lambda EFS example
        15 - Hello World Example With Powertools for AWS Lambda
        16 - DynamoDB Example
        17 - Machine Learning
Template: 1

Use the most popular runtime and package type? (Python and zip) [y/N]: 

Which runtime would you like to use?
        1 - aot.dotnet7 (provided.al2)
        2 - dotnet8
        3 - dotnet6
        4 - go1.x
        5 - go (provided.al2)
        6 - go (provided.al2023)
        7 - graalvm.java11 (provided.al2)
        8 - graalvm.java17 (provided.al2)
        9 - java21
        10 - java17
        11 - java11
        12 - java8.al2
        13 - nodejs20.x
        14 - nodejs18.x
        15 - nodejs16.x
        16 - python3.9
        17 - python3.8
        18 - python3.12
        19 - python3.11
        20 - python3.10
        21 - ruby3.2
        22 - rust (provided.al2)
        23 - rust (provided.al2023)
Runtime: 19

What package type would you like to use?
        1 - Zip
        2 - Image
Package type: 1

Based on your selections, the only dependency manager available is pip.
We will proceed copying the template using pip.

Would you like to enable X-Ray tracing on the function(s) in your application?  [y/N]: 

Would you like to enable monitoring using CloudWatch Application Insights?
For more info, please view https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch-application-insights.html [y/N]: 

Would you like to set Structured Logging in JSON format on your Lambda functions?  [y/N]: 

Project name [sam-app]: sam-app4

    -----------------------
    Generating application:
    -----------------------
    Name: sam-app4
    Runtime: python3.11
    Architectures: x86_64
    Dependency Manager: pip
    Application Template: hello-world
    Output Directory: .
    Configuration file: sam-app4/samconfig.toml
    
    Next steps can be found in the README file at sam-app4/README.md
        

Commands you can use next
=========================
[*] Create pipeline: cd sam-app4 && sam pipeline init --bootstrap
[*] Validate SAM template: cd sam-app4 && sam validate
[*] Test Function in the Cloud: cd sam-app4 && sam sync --stack-name {stack-name} --watch
```

## Sam deploy

```bash
@take-takashi ➜ /workspaces/try-sam/sam-app4 (main) $ sam build
Starting Build use cache                                                                                                                                               
Manifest file is changed (new hash: 3298f13049d19cffaa37ca931dd4d421) or dependency folder (.aws-sam/deps/921600e7-ef53-48ab-8ec6-101279afb2d1) is missing for         
(HelloWorldFunction), downloading dependencies and copying/building source                                                                                             
Building codeuri: /workspaces/try-sam/sam-app4/hello_world runtime: python3.11 metadata: {} architecture: x86_64 functions: HelloWorldFunction                         
 Running PythonPipBuilder:CleanUp                                                                                                                                      
 Running PythonPipBuilder:ResolveDependencies                                                                                                                          
 Running PythonPipBuilder:CopySource                                                                                                                                   
 Running PythonPipBuilder:CopySource                                                                                                                                   

Build Succeeded

Built Artifacts  : .aws-sam/build
Built Template   : .aws-sam/build/template.yaml

Commands you can use next
=========================
[*] Validate SAM template: sam validate
[*] Invoke Function: sam local invoke
[*] Test Function in the Cloud: sam sync --stack-name {{stack-name}} --watch
[*] Deploy: sam deploy --guided


@take-takashi ➜ /workspaces/try-sam/sam-app4 (main) $ sam deploy -g

Configuring SAM deploy
======================

        Looking for config file [samconfig.toml] :  Found
        Reading default arguments  :  Success

        Setting default arguments for 'sam deploy'
        =========================================
        Stack Name [sam-app4]: 
        AWS Region [ap-northeast-1]: 
        #Shows you resources changes to be deployed and require a 'Y' to initiate deploy
        Confirm changes before deploy [Y/n]: 
        #SAM needs permission to be able to create roles to connect to the resources in your template
        Allow SAM CLI IAM role creation [Y/n]: 
        #Preserves the state of previously provisioned resources when an operation fails
        Disable rollback [y/N]: 
        HelloWorldFunction has no authentication. Is this okay? [y/N]: y
        Save arguments to configuration file [Y/n]: 
        SAM configuration file [samconfig.toml]: 
        SAM configuration environment [default]: 

        Looking for resources needed for deployment:

        Managed S3 bucket: aws-sam-cli-managed-default-samclisourcebucket-vurujwdlvecn
        A different default S3 bucket can be set in samconfig.toml and auto resolution of buckets turned off by setting resolve_s3=False
                                                                                                                                                                       
        Parameter "stack_name=sam-app4" in [default.deploy.parameters] is defined as a global parameter [default.global.parameters].                                   
        This parameter will be only saved under [default.global.parameters] in /workspaces/try-sam/sam-app4/samconfig.toml.                                            

        Saved arguments to config file
        Running 'sam deploy' for future deployments will use the parameters saved above.
        The above parameters can be changed by modifying samconfig.toml
        Learn more about samconfig.toml syntax at 
        https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-config.html

        Uploading to sam-app4/e445ae860fe0e8f4a5d56c251fd2a5b9  549486 / 549486  (100.00%)

        Deploying with following values
        ===============================
        Stack name                   : sam-app4
        Region                       : ap-northeast-1
        Confirm changeset            : True
        Disable rollback             : False
        Deployment s3 bucket         : aws-sam-cli-managed-default-samclisourcebucket-vurujwdlvecn
        Capabilities                 : ["CAPABILITY_IAM"]
        Parameter overrides          : {}
        Signing Profiles             : {}

Initiating deployment
=====================

        Uploading to sam-app4/8f1a034a637402e27d659f2228e4c298.template  1342 / 1342  (100.00%)


Waiting for changeset to be created..

CloudFormation stack changeset
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
Operation                                LogicalResourceId                        ResourceType                             Replacement                            
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
+ Add                                    HelloWorldFunctionHelloWorldPermission   AWS::Lambda::Permission                  N/A                                    
+ Add                                    HelloWorldFunctionRole                   AWS::IAM::Role                           N/A                                    
+ Add                                    HelloWorldFunction                       AWS::Lambda::Function                    N/A                                    
+ Add                                    MyApiProdStage                           AWS::ApiGatewayV2::Stage                 N/A                                    
+ Add                                    MyApi                                    AWS::ApiGatewayV2::Api                   N/A                                    
-----------------------------------------------------------------------------------------------------------------------------------------------------------------


Changeset created successfully. arn:aws:cloudformation:ap-northeast-1:878785745404:changeSet/samcli-deploy1711210361/f9485681-749f-47af-9b67-64a185abfec0


Previewing CloudFormation changeset before deployment
======================================================
Deploy this changeset? [y/N]: y

2024-03-23 16:13:06 - Waiting for stack create/update to complete

CloudFormation events from stack operations (refresh every 5.0 seconds)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
ResourceStatus                           ResourceType                             LogicalResourceId                        ResourceStatusReason                   
-----------------------------------------------------------------------------------------------------------------------------------------------------------------
CREATE_IN_PROGRESS                       AWS::CloudFormation::Stack               sam-app4                                 User Initiated                         
CREATE_IN_PROGRESS                       AWS::IAM::Role                           HelloWorldFunctionRole                   -                                      
CREATE_IN_PROGRESS                       AWS::IAM::Role                           HelloWorldFunctionRole                   Resource creation Initiated            
CREATE_COMPLETE                          AWS::IAM::Role                           HelloWorldFunctionRole                   -                                      
CREATE_IN_PROGRESS                       AWS::Lambda::Function                    HelloWorldFunction                       -                                      
CREATE_IN_PROGRESS                       AWS::Lambda::Function                    HelloWorldFunction                       Resource creation Initiated            
CREATE_IN_PROGRESS                       AWS::Lambda::Function                    HelloWorldFunction                       Eventual consistency check initiated   
CREATE_IN_PROGRESS                       AWS::ApiGatewayV2::Api                   MyApi                                    -                                      
CREATE_COMPLETE                          AWS::Lambda::Function                    HelloWorldFunction                       -                                      
CREATE_IN_PROGRESS                       AWS::ApiGatewayV2::Api                   MyApi                                    Resource creation Initiated            
CREATE_COMPLETE                          AWS::ApiGatewayV2::Api                   MyApi                                    -                                      
CREATE_IN_PROGRESS                       AWS::Lambda::Permission                  HelloWorldFunctionHelloWorldPermission   -                                      
CREATE_IN_PROGRESS                       AWS::ApiGatewayV2::Stage                 MyApiProdStage                           -                                      
CREATE_IN_PROGRESS                       AWS::ApiGatewayV2::Stage                 MyApiProdStage                           Resource creation Initiated            
CREATE_IN_PROGRESS                       AWS::Lambda::Permission                  HelloWorldFunctionHelloWorldPermission   Resource creation Initiated            
CREATE_COMPLETE                          AWS::ApiGatewayV2::Stage                 MyApiProdStage                           -                                      
CREATE_COMPLETE                          AWS::Lambda::Permission                  HelloWorldFunctionHelloWorldPermission   -                                      
CREATE_COMPLETE                          AWS::CloudFormation::Stack               sam-app4                                 -                                      
-----------------------------------------------------------------------------------------------------------------------------------------------------------------

CloudFormation outputs from deployed stack
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Outputs                                                                                                                                                            
--------------------------------------------------------------------------------------------------------------------------------------------------------------------
Key                 HelloWorldFunctionIamRole                                                                                                                      
Description         Implicit IAM Role created for Hello World function                                                                                             
Value               arn:aws:iam::878785745404:role/sam-app4-HelloWorldFunctionRole-2h4GV2yFzgdV                                                                    

Key                 HelloWorldApi                                                                                                                                  
Description         API Gateway endpoint URL for Prod stage for Hello World function                                                                               
Value               https://qrjgplzrlb.execute-api.ap-northeast-1.amazonaws.com/Prod/hello/                                                                        

Key                 HelloWorldFunction                                                                                                                             
Description         Hello World Lambda Function ARN                                                                                                                
Value               arn:aws:lambda:ap-northeast-1:878785745404:function:sam-app4-HelloWorldFunction-foczqNO9XjqL                                                   
--------------------------------------------------------------------------------------------------------------------------------------------------------------------


Successfully created/updated stack - sam-app4 in ap-northeast-1
```

### Try

- `Api`を`HttpApi`に変更
- ここで`deply`したら動いた。ただし、発行されたURLの最後の`/`は消さないとダメ
- TODO これに基本認証をかける

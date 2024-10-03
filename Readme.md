# My First AWS Lambda (C# and .NET6)

## 1. Install and Run Visual Studio 2022 Community Edition

We first donwload and install Visual Studio 2022 Community Edition

https://visualstudio.microsoft.com/vs/community/

![image](https://github.com/user-attachments/assets/c924b4fe-f80a-4bba-ac85-4dbce9f62720)

We also have to download and install AWS Toolkit for Visual Studio

https://aws.amazon.com/visualstudio/

![image](https://github.com/user-attachments/assets/8ac5e7b4-364f-4974-ae65-dc7c29fb7771)

![image](https://github.com/user-attachments/assets/52cc4114-d0d6-4b4a-b1b1-793b9f419ac4)

## 2. Create a new Project 

![image](https://github.com/user-attachments/assets/ae2413c2-2c6b-4299-a39d-412aaed83669)

## 3. Select the AWS Lambda projects template 

There are four projects templates for creating AWS Lambda and AWS Serverless applications:

a) AWS Lambda Project (.NET Core - C#)

b) AWS Lambda Project with Tests (.NET Core - C#)

c) AWS Serverless Application (.NET Core - C#)

d) AWS Serverless Application with Tests (.NET Core - C#)

## 4. We are going to select the AWS Lambda Project with Tests (.NET Core - C#)

![image](https://github.com/user-attachments/assets/7f83338c-ef7c-4105-8ec8-e6112510aaeb)

![image](https://github.com/user-attachments/assets/b942b04e-9cb0-4bbf-a435-17e95d9a348a)

![image](https://github.com/user-attachments/assets/54f734b0-b0ce-4395-b816-cc664b407cbd)

This is the project folders and files structure: 

![image](https://github.com/user-attachments/assets/33399d20-987e-4d04-97d0-457f4ddf5421)

Thi is the **Function.cs** code:

```csharp
using Amazon.Lambda.Core;

// Assembly attribute to enable the Lambda function's JSON input to be converted into a .NET class.
[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]

namespace AWSLambdaHelloWorld;

public class Function
{   
    /// <summary>
    /// A simple function that takes a string and does a ToUpper
    /// </summary>
    public string FunctionHandler(string input, ILambdaContext context)
    {
        return input.ToUpper();
    }
}
```

Also we can test the above function with the **FunctionTest.cs** code:

```csharp
using Xunit;
using Amazon.Lambda.Core;
using Amazon.Lambda.TestUtilities;

namespace AWSLambdaHelloWorld.Tests;

public class FunctionTest
{
    [Fact]
    public void TestToUpperFunction()
    {

        // Invoke the lambda function and confirm the string was upper cased.
        var function = new Function();
        var context = new TestLambdaContext();
        var upperCase = function.FunctionHandler("hello world", context);

        Assert.Equal("HELLO WORLD", upperCase);
    }
}
```

## 5. Pay attentionin the following issue

First of all take care when you set the .NET version in the AWS Lambda project. We selected .NET 6 as you can validate in these files:

**AWSLambdaHelloWorld.csproj**

```csharp
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net6.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
    <GenerateRuntimeConfigurationFiles>true</GenerateRuntimeConfigurationFiles>
    <AWSProjectType>Lambda</AWSProjectType>
    <!-- This property makes the build directory similar to a publish directory and helps the AWS .NET Lambda Mock Test Tool find project dependencies. -->
    <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
    <!-- Generate ready to run images during publishing to improve cold start time. -->
    <PublishReadyToRun>true</PublishReadyToRun>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Amazon.Lambda.Core" Version="2.2.0" />
    <PackageReference Include="Amazon.Lambda.Serialization.SystemTextJson" Version="2.4.0" />
  </ItemGroup>
</Project>
```

**aws-lambda-tools-defaults.json**

```csharp
{
  "Information": [
    "This file provides default values for the deployment wizard inside Visual Studio and the AWS Lambda commands added to the .NET Core CLI.",
    "To learn more about the Lambda commands with the .NET Core CLI execute the following command at the command line in the project root directory.",
    "dotnet lambda help",
    "All the command line options for the Lambda command can be specified in this file."
  ],
  "profile": "default",
  "region": "eu-west-3",
  "configuration": "Release",
  "function-architecture": "x86_64",
  "function-runtime": "dotnet6",
  "function-memory-size": 256,
  "function-timeout": 30,
  "function-handler": "AWSLambdaHelloWorld::AWSLambdaHelloWorld.Function::FunctionHandler"
}
```

## 6. How to build and test the application from the command line

We can deploy the application using **Amazon.Lambda.Tools Global Tool** from the command line

https://github.com/aws/aws-extensions-for-dotnet-cli#aws-lambda-amazonlambdatools

Install Amazon.Lambda.Tools Global Tools if not already installed.

```
dotnet tool install -g Amazon.Lambda.Tools
```

If already installed check if new version is available.

```
dotnet tool update -g Amazon.Lambda.Tools
```

**Execute unit tests**

```
cd "AWSLambdaHelloWorld/test/AWSLambdaHelloWorld.Tests"
dotnet test
```

**Deploy function to AWS Lambda**

```
cd "AWSLambdaHelloWorld/src/AWSLambdaHelloWorld"
dotnet lambda deploy-function
```

## 7. How to build and test the application from the Visual Studio IDE

First you need to open Test Explorer 

![image](https://github.com/user-attachments/assets/928cd6a2-4a77-4c7e-a40e-4881abfe0409)

Then run the test case and see the result

![image](https://github.com/user-attachments/assets/6ddddec3-c8d3-4ee7-8634-217b94e376c2)

Also we can deploy the AWS Lambda from Visual Studio 2022, we select the menu option **Publish to AWS Lambda**

![image](https://github.com/user-attachments/assets/ce9e77e7-dfb8-43d6-92b2-1faf842a75e1)

Then we input the required data for publishing

![image](https://github.com/user-attachments/assets/8fb8f9eb-b709-4a70-a3c7-e22aaf82bff6)

![image](https://github.com/user-attachments/assets/6cc7ac45-9db9-4b11-9fe1-73a8cfdaf2c8)

![image](https://github.com/user-attachments/assets/fb8452ee-8cc5-4aab-96b8-9717b8844226)

We can test it see the output

![image](https://github.com/user-attachments/assets/eba0fd7b-c803-4d04-a149-6c3dcff5b38d)


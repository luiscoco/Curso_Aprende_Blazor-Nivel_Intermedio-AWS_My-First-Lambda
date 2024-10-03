# My First AWS Lambda (C# and .NET6)

## 1. Run Visual Studio 2022 Community Edition

We first donwload and install Visual Studio 2022 Community Edition

https://visualstudio.microsoft.com/vs/community/

![image](https://github.com/user-attachments/assets/c924b4fe-f80a-4bba-ac85-4dbce9f62720)

We also have to download and install AWS Toolkit for Visual Studio

https://aws.amazon.com/visualstudio/

![image](https://github.com/user-attachments/assets/8ac5e7b4-364f-4974-ae65-dc7c29fb7771)

![image](https://github.com/user-attachments/assets/52cc4114-d0d6-4b4a-b1b1-793b9f419ac4)

## 2. Create a new Project 

![image](https://github.com/user-attachments/assets/ae2413c2-2c6b-4299-a39d-412aaed83669)

## 3. Select the AWS Lambda project template 

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

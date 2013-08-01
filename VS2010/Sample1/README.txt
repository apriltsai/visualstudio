
This is a simple C# program that has SQL injection vulnerabilities. You need to
have Microsoft Visual Studio 2010 and the .NET Framework 4 installed.

To run this sample, please follow the steps:

1. Open Sample1\Sample1.sln in Microsoft Visual Studio 2010.

2. Do Build->Build Solution.

3. You should see the executable Sample1\Sample1.exe and Debug outputs.

4. Start a "Visual Studio Command Prompt (2010)" window, change to the Sample1 directory and run:

$ sourceanalyzer -b cs-sample -clean
$ sourceanalyzer -b cs-sample -vsversion 10.0 .
$ sourceanalyzer -b cs-sample -scan

Alternatively, if you have the Visual Studio 2010 SCA plugin installed, you can
build and scan the project from the command line:

1. Start up a "Visual Studio Command Prompt (2010)" window.  

2. Change to this directory (VS2010\Sample1) and run the following commands:

$ sourceanalyzer -b cs-sample -clean
$ sourceanalyzer -b cs-sample devenv Sample1.sln /REBUILD Debug
$ sourceanalyzer -b cs-sample -scan

Alternatively, if you have MSBuild 4.0 installed, you can build and scan the 
project from the command line:

1. Start up a "Visual Studio Command Prompt (2010)" window.

2. Change to this directory (VS2010\Sample1) and run the following commands:  

$ sourceanalyzer -b cs-sample -clean
$ sourceanalyzer -b cs-sample msbuild Sample1.sln /target:"REBUILD" /p:Configuration="Debug"
$ sourceanalyzer -b cs-sample -scan

If this is the first time you are scanning .NET code, sourceanalyzer will
load all the needed Microsoft .NET framework files into the cache. This can
take several minutes. This is a one time activity.

Upon successful completion of the scan, you should see the SQL Injection
vulnerabilities and one Unreleased Resource vulnerability. Other categories
may also be present, depending on the rule packs used to scan.

The first SQL Injection vulnerability is on line 29 of Class1.cs where an
argument to the main function is used to create a SQlDataAdapter object.
The second vulnerability shows the tainted data flow through String
concatenation and cloning to a call to create a SqlDataAdapter.

The Unreleased Resource vulnerability is reported for the SQLConnection
object held by the variable 'conn'. The program initializes this variable
with a SQLConnection resource, but never calls the 'Dispose' method on that.
According to the C# API documentation, you should call the 'Dispose' method
on these type of objects so that resources are released properly.

1. Add a new Test.

In benchmark-targets.json
```
{
	"Name": "Assess_CoreWebApi",
	"Class": "PortingAssistant.Client.CLI.Program",
	"Method": "Main",
	"MethodIsAsync": false,
	"MethodParameters": [
		{
			"Name": "Porting",
			"FullTypeName": "string[]",
			"JsonValues": [
				"[\u0022assess\u0022, \u0022-s\u0022, \u0022c:\\\\temp\\\\CoreWebApi\\\\CoreWebApi.sln\u0022, \u0022-o\u0022, \u0022c:\\\\temp\\\\CoreWebApi_output\u0022]"
			] 
		}
	]
},
```

_NOTE:_ Make sure `c:\temp\CoreWebApi` and `c:\temp\CoreWebApi_output` exists!!

_NOTE:_ Technically, we need to update the _benchmark-aws-cofnig.json_ and update `InitializationPowershellScript` to copy those directories as well:

```
mv c:\\\\temp\\\\TestProjects-master\\\\netcoureapp3.1\\\\CoreWebApi c:\\\\temp\\\\CoreWebApi; mkdir c:\\\\temp\\\\CoreWebApi_output;
```

2. Run locally.

```
cd C:\projects\aws\porting-assistant\porting-assistant-dotnet-client

dotnet tool restore

 dotnet benchmark run local managed .\src\PortingAssistant.Client\PortingAssistant.Client.CLI.csproj --config .\benchmark-targets.json -o . --tag main

```

3. Make a change (negative)

```
 public static void Main(string[] args)
	{
		System.Threading.Thread.Sleep(8000);
```

4. Run locally with regression check

```
dotnet benchmark run local managed .\src\PortingAssistant.Client\PortingAssistant.Client.CLI.csproj --config .\benchmark-targets.json -o . --tag newWork --baseline main --threshold 5
```

Show it failed

Also execute

```
$LASTEXITCODE
```

CICD would therefor fail.

5. Show the Reports

6. Show SpeedScope and NetTrace files
  Inside studio and https://www.speedscope.app/

7. Show the change in GitHub
8. BONUS: Show the change in Code Build (requires tweaking the buildspec.yml)
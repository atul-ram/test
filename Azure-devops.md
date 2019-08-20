az configure --defaults location=westus2 group=MyResourceGroup

az devops configure --defaults organization=https://dev.azure.com/atulsahu/ 
az devops configure --defaults project=phippyandfriends

$az devops login --organization https://dev.azure.com/contoso
Token:


# Pipeline


```bash
C:\Windows\system32>az pipelines release list
ID    Name       Definition Name    Created By    Created On                  Status    Description
----  ---------  -----------------  ------------  --------------------------  --------  -------------
8     Release-1  Phippy             atul.sahu     2019-02-21 21:07:08.260000  active
7     Release-1  NodeBrady          atul.sahu     2019-02-21 21:06:26.480000  active
6     Release-1  CaptainKube        atul.sahu     2019-02-21 20:53:23.537000  active
5     Release-4  Parrot-deploy      atul.sahu     2019-02-21 20:51:15.440000  active
4     Release-3  Parrot-deploy      atul.sahu     2019-02-21 20:33:57.140000  active
```
az pipelines release show --id 8

az pipelines release show --id 8  --query variables -o json

az pipelines release show --id 8  --query variables.registryLogin.value  -o json
"fd9be49f-bf93-444e-9357-e2bd4e47e2a9"

az pipelines release show --id 9  --query environments[*].variables   -o json

## Get pipeline name ,path & id
az pipelines release list --output json --query "[].releaseDefinition.[name, path, id]"
## variables by release
az pipelines release show --id 10 --output json  --query " variables"

az pipelines release list --output json  --query "[?contains(name,'ABC')].releaseDefinition.id "

## variables by stage
az pipelines release show --id 9  --query "environments[*].[name, variables]

## variables where stage name = Development
az pipelines release show --id 10 --output json  --query "environments[?name == 'Development'].variables"

## variables, variable group where stage name = Development
```
az pipelines release show --id 11 --output json --query "environments[?name == 'Development'].[name, variables, variableGroups[].name] "
```
```bash

C:\Windows\system32>az pipelines release show --id 10 --output json  --query "environments[*].[name, variables] "
[
  [
    "Development",
    {
      "resourcegroupName": {
        "allowOverride": null,
        "isSecret": null,
        "value": "atul02-d-rg"
      }
    }
  ],
  [
    "Production",
    {
      "resourcegroupName": {
        "allowOverride": null,
        "isSecret": null,
        "value": "atul02-p-rg"
      }
    }
  ]
]
C:\Windows\system32>az pipelines release show --id 10 --output json  --query "environments[?name == 'Development'].variables"
[
  {
    "resourcegroupName": {
      "allowOverride": null,
      "isSecret": null,
      "value": "atul02-d-rg"
    }
  }
]

C:\Windows\system32>az pipelines release show --id 10 --output json  --query " variables"
{
  "resourcegroupName": {
    "allowOverride": null,
    "isSecret": null,
    "value": "atul02-e-rg"
  }
}
```
```
C:\Windows\system32>az pipelines release show --id 8  --query variables -o json
{
  "projectName": {
    "allowOverride": null,
    "isSecret": null,
    "value": "phippy"
  },
  "registryLogin": {
    "allowOverride": null,
    "isSecret": null,
    "value": "fd9be49f-bf93-444e-9357-e2bd4e47e2a9"
  },
  "registryName": {
    "allowOverride": null,
    "isSecret": null,
    "value": "mycontainerregistry9281"
  },
  "registryPassword": {
    "allowOverride": null,
    "isSecret": null,
    "value": "xJyEqekNv2ytsGv110uRkK7WfF8R6vQRKBhBwEPAbBk="
  }
}
```

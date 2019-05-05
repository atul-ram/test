# test
https://docs.microsoft.com/en-us/azure/governance/policy/how-to/programmatically-create



# Login first with az login if not using Cloud Shell

# Get Azure Policy aliases for type Microsoft.Storage
az provider show --namespace Microsoft.Storage --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"

(Get-AzPolicyAlias | Where-Object {$_.Namespace -eq "Microsoft.Storage" -and $_.ResourceType -eq "storageAccounts"}).Aliases | select name | Out-File -FilePath "C:\Users\Atul\All-Storage-Aliases.txt"

# test
$initiativeFiles = @(Get-ChildItem -Path $PolicyFilesPath -File -Recurse -Filter "initiative*.json")
If ($initiativeFiles) {
    $fileCounter = 1
    ForEach ($initiativeFile In $initiativeFiles) {
        Write-Host "Processing policy initiative file $fileCounter/$($initiativeFiles.Count) with name '$($initiativeFile.FullName)'."
        $initiativeSource = Get-Content -Path $initiativeFile.FullName | ConvertFrom-Json

        # Check for common error where the subscription id placeholder has not been replaced yet
        ForEach ($policyDefinitionRef In $initiativeSource.properties.policyDefinitions) {
            If ($policyDefinitionRef.policyDefinitionId.Contains('#{subscriptionId}#')) {
                Write-Error "Cannot proceed as #{subscriptionId}# token has not been replaced yet in '$($policyDefinitionRef.policyDefinitionId)'."
            }
            If ($policyDefinitionRef.policyDefinitionId.Contains('#{')) {
                Write-Error "Detected unreplaced token in parameter '$($policyDefinitionRef.policyDefinitionId)'."
            }

            # Do nothing for an "empty" PSCustomObject, use this construct to test for it.
            If (-not "$($policyDefinitionRef.parameters)") {
                Continue
            }
            
            ForEach ($parametersRef In $policyDefinitionRef.parameters) {
                If ($parametersRef.psobject.properties.name) {
                    ForEach ($parameter In $parametersRef.psobject.properties.name) {
                        $parameterRef = $parametersRef.$parameter
                        If ($parameterRef.Value.Contains('#{')) {
                            Write-Error "Detected unreplaced token in parameter '$($parameterRef.Value)'."
                        }
                    }
                }
            }
        }

        $definitions = $initiativeSource.properties.policyDefinitions | ConvertTo-Json -Depth 50
        $params = $initiativeSource.properties.parameters | ConvertTo-Json -Depth 50

        # This will create the initiative in the current Azure context. This must be subscription scope for now.
        New-AzureRmPolicySetDefinition -DisplayName $initiativeSource.properties.displayName `
                                       -Description $initiativeSource.properties.description `
                                       -Parameter $params `
                                       -PolicyDefinition $definitions `
                                       -Name $initiativeSource.name

        $fileCounter++
    }

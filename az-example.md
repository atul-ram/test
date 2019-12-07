
# check particular sku is available in a region
```bash
az vm list-skus --location westeurope --output json --query "[?name=='Standard_B2s']"
```

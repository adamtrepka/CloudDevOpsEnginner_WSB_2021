
## Polecenie wykorzystane do wykonania ćwiczenia LAB07

### Ustawienie zmiennej środowiskowej
` version=$(az aks get-versions -l westeurope --query 'orchestrators[-1].orchestratorVersion' -o tsv)`
### Utworzenie Resource Group
 `az group create --name akshandsonlab --location westeurope`
### Utowrzenie Azure Kubernetes Service
` az aks create --resource-group akshandsonlab --name labaks --enable-addons monitoring --kubernetes-version $version --generate-ssh-keys --location westeurope --node-vm-size Standard_B2ms --node-count 2`

### Utworzenie Azure Container Registry
 `az acr create --resource-group akshandsonlab --name pw26799acr --sku Standard --location westeurope`

### Podłączenie Azure Kubernetes Service do Azure Container Registry
 `az aks update -n labaks -g akshandsonlab --attach-acr pw26799acr`

### Utworzenie servera SQL
`az sql server create -l westeurope -g akshandsonlab -n pw26799sqlsrv -u sqladmin -p P2ssw0rd1234`

### Utworzenie bazy danych SQL
 `az sql db create -g akshandsonlab -s pw26799sqlsrv -n mhcdb --service-objective S0`

### Adres servera bazy danych
 `SqlServerName: pw26799sqlsrv.database.windows.net`
### Adres Azure Container Registry
 `ContainerRegistry: pw26799acr.azurecr.io`

### Autoryzacja lini komend w usłudze Azure Kubernetes Service 
 `az aks get-credentials --resource-group akshandsonlab --name labaks`

### Pobranie listy "Pod'ów"
` kubectl get pods`
### Pobranie danych loadbalancer'a
 `kubectl get service mhc-front --watch`

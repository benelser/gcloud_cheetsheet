# gcloud_cheetsheet
Frequently use gcloud commands 

## Global Config
```bash
gcloud config get-value project # get current project
gcloud config set project elser-test # set/change current project
```

## Org Level
```bash
gcloud organizations list # List orgs to get id
gcloud alpha resource-manager folders list --folder=123456789 # List all folders under specific folder id
gcloud alpha billing accounts list # Get billing accounts
```
## Org Level Policy Management
```bash
gcloud resource-manager org-policies describe compute.disableSerialPortLogging --effective --organization 695029235726 # Get effective policy value on specific contraint
gcloud resource-manager org-policies enable-enforce compute.disableSerialPortLogging --organization 695029235726 # Set and enable specific contraint
gcloud resource-manager org-policies list --organization 695029235726 # Get all contraints applied at org level. You can supply a --folder or --project instead
gcloud resource-manager org-policies describe compute.disableSerialPortLogging --effective --project product1-prod-a790 # View the inherited "enforced" policy from org level
gcloud resource-manager org-policies disable-enforce compute.disableSerialPortLogging --project product1-prod-a790 # Break inheritance of org policy contraint
```

## Compute 
```bash
gcloud compute instances list # list compute instances "VM's"
```
Get CURRENT authenticated Identity 
```bash
gcloud config list account --format "value(core.account)"
```

## Functions
```bash
gcloud functions list # list cloud functions "lambdas"
```

## Auth
```bash
gcloud auth login # This will take you through OAUTH flow
gcloud auth activate-service-account elserdotnet@product1-prod-b3c0.iam.gserviceaccount.com --keyfile=./key.json # Activates SA
```
### Set env variable for gcloud sdk to use to include tools sitting on top of SDK
You could set this using absolute path to use across tools and jobs. Ensure your .gitignore is ignoring your key file if it is in directory.
##### .gitignore
```
# key
key.json
```
- Windows
```powershell
$env:GOOGLE_APPLICATION_CREDENTIALS="./key.json"
```
- Everything Else
```bash
export GOOGLE_APPLICATION_CREDENTIALS="./key.json"
```

## Projects
```bash
gcloud projects list # if you created a project earlier you should see it listed
gcloud config set project product1-dev-33a8 # set current project context
gcloud config get-value project # get current project
gcloud projects create help # get help listing command FLAGS 
gcloud projects create --help # For more verbose help
gcloud projects create cli-123456 --name bensfirstproject --folder 587805611271 # Creates project with id cli-123456 and name 
```

## Secrets Manager
```bash
gcloud secrets list # list all secrets PROJECT specific context
gcloud secrets versions access latest --secret="<SECRETNAME>" read contents of secret name discoverd with above command
```
## IAM
```bash
gcloud alpha resource-manager folders get-iam-policy <FOLDERID> get iam bindings on folder
```
List roles associated with service account
```powershell
gcloud projects get-iam-policy <project> `
--flatten="bindings[].members" `
--format='table(bindings.role)' `
--filter="bindings.members:<SA EMAIL>"
```
Get CURRENT authenticated Identity 
```bash
gcloud config list account --format "value(core.account)"
```
Impersonate SA -- User must be a member of the roles/iam.serviceAccountTokenCreator on the SA resource.
```bash
gcloud config set auth/impersonate_service_account first-sa@product1-prod-a790.iam.gserviceaccount.com # Activate Impersoination
gcloud config get-value auth/impersonate_service_account # Check current impersonation
```
## Service Accounts
Create
```bash
gcloud iam service-accounts create svc-day1-teamcreated --description="createdByTeam" --display-name="this is my scoped sa for k8"
```
List
```bash
gcloud iam service-accounts list
```

## Accessing metadta
```powershell
$headers = @{
    "Metadata-Flavor" = "Google"
}

$t = iwr -uri "http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default" -Headers $headers
$t.RawContent 
[system.text.encoding]::ASCII.Getstring($t.content)
```

## Grabbing current User and making REST API call
```bash
$header = @{
    "Authorization" = "Bearer $((gcloud auth print-access-token --format=json | ConvertFrom-Json | select token).token)"
}
$r = iwr -uri "https://cloudresourcemanager.googleapis.com/v1/projects" -Headers $header
$r.Content  | ConvertFrom-Json
```

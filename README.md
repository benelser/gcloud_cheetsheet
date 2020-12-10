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

## Compute 
```bash
gcloud compute instances list # list compute instances "VM's"
```

## Functions
```bash
gcloud functions list # list cloud functions "lambdas"
```

## Auth
```bash
gcloud auth login # This will take you through OAUTH flow
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

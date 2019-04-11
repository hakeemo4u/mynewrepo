# Abbott NextGen Infra Privisioning
Terraform script to spin up AWS infrastrucutre / services.
- Required Module Input Variables
  - Service Catalog Modules : Pipe seperated List of AWS services
  `Service_Catalog_Modules = "12s3|13vpc|15securitygroups"`
  - region : Targate AWS Region 
    `region = "eu-west-1"`  
  - Common Tags : Common tags attached to resource created
    `division    = "Healthcare"
    project     = "abbott-next-gen"
    application = "Libre"
    environment = "Development"
    node        = "Primary"
    created_by  = "raghavendra.ck@capgemini.com"`  
  - Remote State : Terraform state file configuration
   `terraform_remote_state_region = "us-east-1"
    terraform_remote_state_bucket = "management-vpc-test-sandbox-terraform"
    terraform_remote_state_file_name = "terraform.tfstate"`
  - Configuration variable per module : Service / modules defined in `Service_Catalog_Modules`
- Optional Module Input Variables
  - Remote State for modules: When output of module is input for another module. For example output of VPC module (Subnet IDS) is required for Compute module to spin up EC2 instances. Define a remote state variable of VPC module and referance it in compute module while creating EC2 instances
  `terraform_remote_state_vpc_key = "13vpc/terraform.tfstate"
   terraform_remote_state_s3_key = "12s3/terraform.tfstate"
   terraform_remote_state_sg_key = "15securitygroups/terraform.tfstate"`
   terraform_remote_state_alb_key = "23alb/terraform.tfstate"`
   terraform_remote_state_cognito_key = "51cognito/terraform.tfstate"`
### Deployment 
  - Checkout terraform-modules (repo URL) in `terraform-modules` folder
  - Checkout bash-scripts (repo URL) `bash-scripts` folder 
  - Checkout environment specific override.tfvar (repo URL) `terraform-environments` folder 
```sh
		echo ENVIRONMENT_NAME = development #['Management','development','stg']
		cd terraform-scripts/scripts
		sh ./bin/Pull_Suplimental_Enviroment_Files.sh $ENVIRONMENT_NAME
		./bin/init_all.sh
		#to spinup environment 
		./bin/apply_all.sh
		#to destroy
		./bin/destroy_all.sh
```

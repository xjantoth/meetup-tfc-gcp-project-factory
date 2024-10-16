### Demo Terraform GCP with Hashicorp Vault

```bash
gcloud config configurations create demo-nocode
gcloud config configurations list
...
demo-nocode  True

gcloud services enable iam.googleapis.com --project mystic-airway-438411-a1

for service in compute.googleapis.com \
iam.googleapis.com \
iamcredentials.googleapis.com \
oslogin.googleapis.com; \
do gcloud services enable ${service} --project gcp-demo-vault-0; done
```

### Export Environmental Variables

```bash
gcloud config set account jan.toth@devopsinuse.sk
gcloud auth application-default login

export GOOGLE_PROJECT="mystic-airway-438411-a1"
export TF_VAR_VAULT_PROJECT_ID="gcp-demo-vault-0"
export GOOGLE_APPLICATION_CREDENTIALS=~/.config/gcloud/application_default_credentials.json

export TF_VAR_project_id="mystic-airway-438411-a1"
export TF_VAR_prefix="demo"
export TF_VAR_secret_engine_name="gcp-demo"
export TF_VAR_default_lease_ttl_seconds="300"
export TF_VAR_max_lease_ttl_seconds="300"

export TF_VAR_tfe_host="app.terraform.io"
export TF_VAR_tfe_org_email="devopsinuse@gmail.com"
export TF_VAR_vcs_tf_module="xjantoth/terraform-gcp-demo-module"

export TF_VAR_tfe_token=$(security find-generic-password -a $USER -s TF_CLOUD_TOKEN -w)
export TF_VAR_vcs_github_token=$(security find-generic-password -a $USER -s GITHUB_TOKEN -w)
```

### Execute terraform code to setup Terrafrom Cloud, Provision Vault in dedicated GCP project and configure Vault itself (secret engine, jwt auth)

```bash
cd tf-setup
terraform init
terraform plan

```

### Usefule commands
```bash
gcloud iam service-accounts keys list --iam-account=demo-impersonator-0@mystic-airway-438411-a1.iam.gserviceaccount.com  --project=mystic-airway-438411-a1
```

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 1.8.3 |
| <a name="requirement_google"></a> [google](#requirement\_google) | >= 5.43.1 |
| <a name="requirement_google-beta"></a> [google-beta](#requirement\_google-beta) | >= 5.43.1 |
| <a name="requirement_random"></a> [random](#requirement\_random) | ~> 3.6.3 |
| <a name="requirement_tfe"></a> [tfe](#requirement\_tfe) | >= 0.59.0 |
| <a name="requirement_vault"></a> [vault](#requirement\_vault) | ~> 4.4.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_google.vault"></a> [google.vault](#provider\_google.vault) | 5.44.1 |
| <a name="provider_random"></a> [random](#provider\_random) | 3.6.3 |
| <a name="provider_tfe"></a> [tfe](#provider\_tfe) | 0.59.0 |
| <a name="provider_vault"></a> [vault](#provider\_vault) | 4.4.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| <a name="module_executor"></a> [executor](#module\_executor) | github.com/GoogleCloudPlatform/cloud-foundation-fabric//modules/iam-service-account | v34.1.0 |
| <a name="module_impersonator"></a> [impersonator](#module\_impersonator) | github.com/GoogleCloudPlatform/cloud-foundation-fabric//modules/iam-service-account | v34.1.0 |
| <a name="module_vault"></a> [vault](#module\_vault) | github.com/GoogleCloudPlatform/cloud-foundation-fabric//modules/iam-service-account | v34.1.0 |

## Resources

| Name | Type |
|------|------|
| [google_compute_firewall.this](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_firewall) | resource |
| [google_compute_instance.default](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_instance) | resource |
| [google_compute_network.this](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_network) | resource |
| [google_compute_subnetwork.this](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_subnetwork) | resource |
| [random_string.this](https://registry.terraform.io/providers/hashicorp/random/latest/docs/resources/string) | resource |
| [tfe_oauth_client.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/oauth_client) | resource |
| [tfe_organization.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/organization) | resource |
| [tfe_project.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/project) | resource |
| [tfe_project_variable_set.test](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/project_variable_set) | resource |
| [tfe_registry_module.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/registry_module) | resource |
| [tfe_variable.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable) | resource |
| [tfe_variable_set.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/variable_set) | resource |
| [tfe_workspace.this](https://registry.terraform.io/providers/hashicorp/tfe/latest/docs/resources/workspace) | resource |
| [vault_gcp_secret_backend.this](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/gcp_secret_backend) | resource |
| [vault_gcp_secret_static_account.this](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/gcp_secret_static_account) | resource |
| [vault_jwt_auth_backend.jwt](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend) | resource |
| [vault_jwt_auth_backend_role.demo_role](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/jwt_auth_backend_role) | resource |
| [vault_policy.policies](https://registry.terraform.io/providers/hashicorp/vault/latest/docs/resources/policy) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_VAULT_PROJECT_ID"></a> [VAULT\_PROJECT\_ID](#input\_VAULT\_PROJECT\_ID) | GCP project where Vault will be deployed. | `string` | n/a | yes |
| <a name="input_default_lease_ttl_seconds"></a> [default\_lease\_ttl\_seconds](#input\_default\_lease\_ttl\_seconds) | Vault default lease in seconds. | `number` | n/a | yes |
| <a name="input_max_lease_ttl_seconds"></a> [max\_lease\_ttl\_seconds](#input\_max\_lease\_ttl\_seconds) | Vault max ttl lease in seconds. | `number` | n/a | yes |
| <a name="input_prefix"></a> [prefix](#input\_prefix) | Prefix applied to service account names. | `string` | `null` | no |
| <a name="input_project_id"></a> [project\_id](#input\_project\_id) | Demo GCP project\_id | `string` | n/a | yes |
| <a name="input_secret_engine_name"></a> [secret\_engine\_name](#input\_secret\_engine\_name) | Vault GCP secret engine name. | `string` | n/a | yes |
| <a name="input_source_ranges"></a> [source\_ranges](#input\_source\_ranges) | Allowed source ranges for Vault instance. | `list(string)` | n/a | yes |
| <a name="input_tfc_address"></a> [tfc\_address](#input\_tfc\_address) | Terraform Cloud address. | `string` | n/a | yes |
| <a name="input_tfe_host"></a> [tfe\_host](#input\_tfe\_host) | Terrafrom Cloud host. | `string` | n/a | yes |
| <a name="input_tfe_org_email"></a> [tfe\_org\_email](#input\_tfe\_org\_email) | Github OAUTH token. | `string` | n/a | yes |
| <a name="input_tfe_token"></a> [tfe\_token](#input\_tfe\_token) | Terrafrom Cloud token. | `string` | n/a | yes |
| <a name="input_variable_sets"></a> [variable\_sets](#input\_variable\_sets) | Organisation variable sets | <pre>map(object({<br>    global      = optional(bool, false)<br>    description = optional(string, "")<br>    workspaces  = optional(list(string), [])<br>    variables = map(object({<br>      value       = string<br>      category    = optional(string, "terraform")<br>      description = optional(string, null)<br>      hcl         = optional(bool, false)<br>      sensitive   = optional(bool, false)<br>    }))<br>  }))</pre> | `{}` | no |
| <a name="input_vcs_github_token"></a> [vcs\_github\_token](#input\_vcs\_github\_token) | Github OAUTH token. | `string` | n/a | yes |
| <a name="input_vcs_tf_module"></a> [vcs\_tf\_module](#input\_vcs\_tf\_module) | Github module path e.g. xjantoth/terraform-gcp-demo-module | `string` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_VAULT_TOKEN"></a> [VAULT\_TOKEN](#output\_VAULT\_TOKEN) | n/a |
| <a name="output_browser_public"></a> [browser\_public](#output\_browser\_public) | Browser URL public |
| <a name="output_browser_tunnel"></a> [browser\_tunnel](#output\_browser\_tunnel) | Browser URL tunnel |
| <a name="output_ssh"></a> [ssh](#output\_ssh) | SSH command copy/paste. |
| <a name="output_tunnel"></a> [tunnel](#output\_tunnel) | SSH command copy/paste. |
<!-- END_TF_DOCS -->
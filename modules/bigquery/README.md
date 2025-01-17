# Log Export: BigQuery destination submodule

This submodule allows you to configure a BigQuery dataset destination that
can be used by the log export created in the root module.

## Usage

The [examples](../../examples) directory contains directories for each destination, and within each destination directory are directories for each parent resource level. Consider the following
example that will configure a BigQuery dataset destination and a log export at the project level:

```hcl
module "log_export" {
  source                 = "terraform-google-modules/log-export/google"
  destination_uri        = "${module.destination.destination_uri}"
  filter                 = "severity >= ERROR"
  log_sink_name          = "bigquery_example_logsink"
  parent_resource_id     = "sample-project"
  parent_resource_type   = "project"
  unique_writer_identity = "true"
}

module "destination" {
  source                   = "terraform-google-modules/log-export/google//modules/bigquery"
  project_id               = "sample-project"
  dataset_name             = "sample_dataset"
  log_sink_writer_identity = "${module.log_export.writer_identity}"
}
```

At first glance that example seems like a circular dependency as each module declaration is
using an output from the other, however Terraform is able to collect and order all the resources
so that all dependencies are met.

<!-- BEGINNING OF PRE-COMMIT-TERRAFORM DOCS HOOK -->
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| dataset\_name | The name of the bigquery dataset to be created and used for log entries matching the filter. | string | n/a | yes |
| default\_table\_expiration\_ms | Default table expiration time (in ms) | string | `"3600000"` | no |
| delete\_contents\_on\_destroy | (Optional) If set to true, delete all the tables in the dataset when destroying the resource; otherwise, destroying the resource will fail if tables are present. | string | `"true"` | no |
| description | A use-friendly description of the dataset | string | `"Log export dataset"` | no |
| labels | Dataset labels | map | `<map>` | no |
| location | The location of the storage bucket. | string | `"US"` | no |
| log\_sink\_writer\_identity | The service account that logging uses to write log entries to the destination. (This is available as an output coming from the root module). | string | n/a | yes |
| project\_id | The ID of the project in which the bigquery dataset will be created. | string | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| console\_link | The console link to the destination bigquery dataset |
| destination\_uri | The destination URI for the bigquery dataset. |
| project | The project in which the bigquery dataset was created. |
| resource\_id | The resource id for the destination bigquery dataset |
| resource\_name | The resource name for the destination bigquery dataset |
| self\_link | The self_link URI for the destination bigquery dataset |

<!-- END OF PRE-COMMIT-TERRAFORM DOCS HOOK -->

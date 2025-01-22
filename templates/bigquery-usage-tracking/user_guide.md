# BigQuery Template for Lightdash

## Prerequisites

1. **Create a dbt Model**  
   Ensure you have a dbt model for Lightdash to reference. This template is based on the `information_schema.jobs` table provided by BigQuery.  

   If you don’t already have one, create a `.sql` file in your dbt project named `big_query_jobs.sql` with the following content:

   ```sql
   {{
     config(
       materialized='table'
     )
   }}

   select * from `region-us`.`information_schema`.`jobs`
   ```

   > **Note**: Replace `region-us` with your region. Refer to the [BigQuery documentation](https://cloud.google.com/bigquery/docs/information-schema-jobs) for details.

2. **Generate a YAML File**  
   Use the Lightdash CLI to generate a YAML file for the BigQuery model:
   ```bash
   lightdash generate -s big_query_jobs
   ```

   Avoid changing any column names for the smoothest experience implementing the template.

   > **Note**: No metrics need to be defined here. Metrics will be created as part of the template.

3. **Deploy your changes to Lightdash**
   The model you've just created needs to exist as a Table in Lightdash for the template to work (otherwise, you'll get an error saying `Explore "big_query_jobs" does not exist.` in your dashboard). Before uploading the charts and dashboard from the template below, you'll need to deploy your new `big_query_jobs` model to Lightdash. 

## Using the Template

Once you have an underlying model that returns your BigQuery jobs information, follow these steps to use the template:

1. **Download the Template**  
   Download the `lightdash` folder from the relevant section in the `lightdash-templates` repository. You can clone the repository by running `git clone git@github.com:lightdash/lightdash-templates.git`.

2. **Copy the Template**  
   Copy the `lightdash` folder into the root of your dbt directory. 

   > If you already have a lightdash content folder in your dbt project, just copy across the charts and dashboards to the relevant folders.

   - *Make the necessary changes to the template* as outlined in the Adjusting the Template section below.
   

3. **Set Up the Lightdash CLI**  
   Ensure you are logged into the Lightdash CLI using `lightdash login`

4. **Upload the Template**
   Navigate to the directory where the `lightdash` folder with the charts and dashboards is located.
   
   Use the following command to upload the BigQuery dashboard and charts:
   ```bash
   lightdash upload --force --dashboards ld-bq-bigquery-usage-tracking --charts ld-bq-jobs-ran ld-bq-top-users ld-bq-jobs-by-statement-type ld-bq-cost ld-bq-billed-gib ld-bq-billed-gib-by-statement-type ld-bq-big-query-usage ld-bq-big-query-costs
   ```
   Optionally, you can also include the argument `--project <uuid1>` to upload directly to a specific project within your organization. If not supplied the current option selected via the CLI will be the target for the new content.

## Adjusting the BigQuery Template

The template consists of a dashboard file and multiple chart files. Before uploading, you need to make the following adjustments:

- Replace commonly changed fields. For BigQuery that usually means the amount charged per TiB of compute, or the currency formatting. The default is 6.5USD per TiB. The following model files can be adjusted to accomodate:

    - `ld-bq-top-users.yml`
    - `ld-bq-cost.yml`
    - `ld-bq-big-query-costs.yml`

For each of these files, replace the value `6.5` with whatever your compute per TiB is charged at. You can find docs on this through Google [here](https://cloud.google.com/bigquery/pricing#analysis_pricing_models) 

You can also update the field formatting under this value from `USD` to `GBP` or `EUR` as appropriate.

- **Update column references if you have renamed columns in the BigQuery `information_schema.jobs` model**. Use a find-and-replace tool to update all files.

### Optional Adjustments
You can customize other settings in the template YAML files, such as default filters. However, it might be easier to make these changes directly in the Lightdash UI.

# Snowflake Template for Lightdash

## Prerequisites

1. **Create a dbt Model**
   Ensure you have a dbt model for Lightdash to reference. This template is based on the `QUERY_HISTORY` view provided by Snowflake's ACCOUNT_USAGE schema.

   If you don't already have one, create a `.sql` file in your dbt project named `snowflake_query_history.sql` with the following content:

   ```sql
   {{
     config(
       materialized='table'
     )
   }}

   select * from snowflake.account_usage.query_history
   ```

   > **Note**: You can also use the `INFORMATION_SCHEMA.QUERY_HISTORY` table function for more recent queries. Refer to the [Snowflake documentation](https://docs.snowflake.com/user-guide/ui-snowsight-activity#viewing-query-details-and-results) for details.

2. **Generate a YAML File**
   Use the Lightdash CLI to create the table and generate a YAML file for the Snowflake model:
   ```bash
   lightdash dbt run -s snowflake_query_history
   ```

   Avoid changing any column names for the smoothest experience implementing the template.

   > **Note**: No metrics need to be defined here. Metrics will be created as part of the template.

3. **Deploy your changes to Lightdash**
   The model you've just created needs to exist as a Table in Lightdash for the template to work (otherwise, you'll get an error saying `Explore "snowflake_query_history" does not exist.` in your dashboard). Before uploading the charts and dashboard from the template below, you'll need to deploy your new `snowflake_query_history` model to Lightdash.

## Using the Template

Once you have an underlying model that returns your Snowflake query history information, follow these steps to use the template:

1. **Download the Template**
   Download the `snowflake` folder from the `templates/usage-tracking/` section in the `lightdash-templates` repository. You can clone the repository by running `git clone git@github.com:lightdash/lightdash-templates.git`.

2. **Copy the Template**
   Copy the `lightdash` folder from `templates/usage-tracking/snowflake/lightdash` into the root of your dbt directory.

   > If you already have a lightdash content folder in your dbt project, just copy across the charts and dashboards to the relevant folders.

   - *Make the necessary changes to the template* as outlined in the Adjusting the Template section below.


3. **Set Up the Lightdash CLI**
   Ensure you are logged into the Lightdash CLI using `lightdash login`

4. **Upload the Template**
   Navigate to the directory where the `lightdash` folder with the charts and dashboards is located.

   Use the following command to upload the Snowflake dashboard and charts:
   ```bash
   lightdash upload --force --dashboards ld-sf-snowflake-usage-tracking --charts ld-sf-queries-ran ld-sf-top-users ld-sf-queries-by-statement-type ld-sf-credits-used ld-sf-bytes-scanned ld-sf-bytes-scanned-by-statement-type ld-sf-snowflake-usage ld-sf-snowflake-costs
   ```
   Optionally, you can also include the argument `--project <uuid1>` to upload directly to a specific project within your organization. If not supplied the current option selected via the CLI will be the target for the new content.

5. **Open Lightdash to see your new dashboard**
   Your new dashboard for Snowflake usage (`Snowflake Usage Tracking`) should now appear in your Lightdash project in a new space called `Templates`. Magic.

## Adjusting the Snowflake Template

The template consists of a dashboard file and multiple chart files. Before uploading, you need to make the following adjustments:

- Replace commonly changed fields. For Snowflake that usually means the credit pricing or currency formatting. The default credit cost can vary based on your Snowflake edition and region. The following model files can be adjusted to accommodate:

    - `ld-sf-top-users.yml`
    - `ld-sf-credits-used.yml`
    - `ld-sf-snowflake-costs.yml`

For each of these files, replace the credit cost value with your actual Snowflake credit pricing. You can find docs on this through Snowflake [here](https://www.snowflake.com/pricing/)

You can also update the field formatting from `USD` to `GBP` or `EUR` as appropriate.

- **Update column references if you have renamed columns in the Snowflake `query_history` model**. Use a find-and-replace tool to update all files.

### Optional Adjustments
You can customize other settings in the template YAML files, such as default filters. However, it might be easier to make these changes directly in the Lightdash UI.

## Understanding Snowflake Query History Data

The query history in Snowflake includes comprehensive metadata:

- **Execution information**: Query status, start/end times, execution duration, warehouse used
- **Performance metrics**: Bytes scanned, rows returned, query ID, compilation time
- **Cost factors**: Warehouse size, credits consumed, session ID, query tag
- **User details**: User who submitted the query, client application and version

> **Note**: Query details in Snowsight are preserved for 14 days. Queries executed more than 7 days ago may not include user information.

## Access Requirements

To access query history data, you need appropriate privileges:

- **ACCOUNTADMIN** role: Full access to all account query history
- **MONITOR** privilege: Access to queries for specific warehouses or users
- **GOVERNANCE_VIEWER** role: Access via SQL queries only

Ensure your service account or user has the necessary permissions to query the `ACCOUNT_USAGE.QUERY_HISTORY` view.

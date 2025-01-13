# Events Explorer Template for Lightdash

## Prerequisites

1. **Create a dbt Model**  
   Ensure you have a dbt model for Lightdash to reference. This template is based on event streams that you might retrieve from 'track' events tables such as the ones created by [Rudderstack](https://www.rudderstack.com/docs/event-spec/standard-events/track/) or [Segment](https://segment.com/docs/connections/storage/warehouses/schema/#tracks-table). It is assumed you are querying a table that has one row per event fired, and the template is most useful when a table with many different events is selected.

   If you don’t already have one, create a `.sql` file in your dbt project named `events.sql` following the below pattern:

   ```sql
   {{
     config(
       materialized='table'
     )
   }}

   select  
        -- note that the below column names are the defaults within the template.
        -- if you have different column names, you'll need to adjust the template before uploading.
        id,
        user_id,
        event,
        timestamp
   from {{ ref('your_raw_events_table') }}
   ```
   > **Note**: It's fine to add more columns here! The template is only based on the four above, but won't break if you have more to facilitate further exploration.

2. **Generate a YAML File**  
   Use the Lightdash CLI to generate a YAML file for the `events` model:
   ```bash
   lightdash generate -s events
   ```

   > **Note**: No metrics *need* to be defined here. Metrics will be created as part of the template.

## Using the Template

Once you have an underlying model that returns your Events data, follow these steps to use the template:

1. **Download the Template**  
   Download the `lightdash` folder from the relevant section in the `lightdash-templates` repository.

2. **Copy the Template**  
   Copy the `lightdash` folder into the root of your dbt directory. 

   > If you already have a lightdash content folder in your dbt project, just copy across the charts and dashboards to the relevant folders.

   - *Make the necessary changes to the template* as outlined in the Adjusting the Template section below. There is one change that is ⚠️ **compulsory** ⚠️ for all users.

3. **Set Up the Lightdash CLI**  
   Ensure you are logged into the Lightdash CLI using `lightdash login`

4. **Upload the Template**  
   Use the following command to upload the BigQuery dashboard and charts:
   ```bash
   lightdash upload --force -dashboards ld-ee-events-explorer -charts ld-ee-event-counts-by-popularity ld-ee-event-counts ld-ee-user-counts ld-ee-user-funnel
   ```
   Optionally, you can also include the argument `--project <uuid1>` to upload directly to a specific project within your organization. If not supplied the current option selected via the CLI will be the target for the new content.

## Adjusting the Events Template

The template consists of a dashboard file and multiple chart files. Before uploading, you need to make the following adjustments:

- **Replace the events that you wish to show in the events funnel.** Navigate to the `ld-ee-user-funnel.yml` file and change the records under the 'values' key. Note these will be ordered descending base on the user count.

- **Update column references in your events model if you have different names than the template default**. Use a find-and-replace tool to update all files. Pay close attention here as there are instances where you may want to replace just the string `event` but not the string `events`.

### Optional Adjustments
You can customize other settings in the template YAML files, such as default filters. However, it might be easier to make these changes directly in the Lightdash UI.
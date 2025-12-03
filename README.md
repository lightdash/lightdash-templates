# Lightdash Templates 🚀

Accelerate your analytics with pre-built **Lightdash** content. ⚡️

[Lightdash](https://github.com/lightdash/lightdash) is an open-source BI tool—learn more about it in our [docs](https://docs.lightdash.com). 🔍

The templates in this repo extend the [Content as Code](https://docs.lightdash.com/references/content-as-code/) functionality, making your journey from source data to insights even faster.

## Community Templates

If you have content that could benefit the wider Lightdash community, please feel empowered to open a PR and share it here! 🙌 A few tips:

- **Minimize extra transformations:** Stick as close to source data as possible, since everyone’s data modeling can differ. 🔧
- If you must do some modeling, **clearly document** it so others can adapt it. 📝
- Build you dashboards and charts in the Lightdash UI, then use `lightdash download` to get a draft template, and finally generalise and simplify!

## How to

This repo is organized by data type or domain (e.g., bigquery-usage-tracking). Each folder contains specific instructions, but here’s the gist:

1. **Navigate** to the relevant folder (e.g., *BigQuery*) and review the included charts/dashboards. 📂
2. **Copy** the yml files into a `lightdash` folder in the root of your dbt project. 📑
3. If needed, **create** new dbt models/yml files for the underlying data. BigQuery, for example, may require a model pointed at `information_schema.jobs`. _Tip - `lightdash generate` will save you time here!_ ✨
4. **Adjust** the content yml references to point to your own tables. ⚙️
5. Finally, **run** `lightdash upload --force` to immediately push your new content to Lightdash. 🚀

## Content as code 

For more details on how to extend your dashboards, metrics, or charts through version control, check out the [Dashboards as Code docs](https://docs.lightdash.com/references/dashboards-as-code). Happy templating! 🎉

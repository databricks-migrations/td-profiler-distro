# TD Profiler
>### Usage Instructions:
<br>

**TD Profiler Notebooks :**

* Notebooks are packaged as zip files and avialable under [td-profiler-repo Releases](https://github.com/databricks-migrations/td-profiler-distro/releases) for users to import them directly in to their Databricks workspace.
<img src="documentation/assets/td-profiler-distro-page-sample.png" alt="TD Profiler Notebooks Distribution" width="600"/>
* Always import from latest release tag 


**Requried Cluster Configuration:**
- It is recomended to run the Teradata Usage data extract notebook in dedicated cluster
- Create a new cluster with **Standard** Cluster Mode (Scala support is required for Notebook execution) and two worker Nodes 
- Install the JDBC Driver jar library on the cluster. Refere [Cluster libraries](https://docs.databricks.com/libraries/cluster-libraries.html#cluster-libraries) section of Databricks documentation if you are haven't installed cluster libraries before.


**Import Notebooks into Workspace:**

* Create new folder with name **td-profiler** in Databricks Workspace
* Import latest version of Profiler Notebooks from above distribution link into the **td-profiler** folder.
* Ref [Import an archive](https://docs.databricks.com/notebooks/notebooks-manage.html#import-an-archive) section of Databricks documentation for detail steps. Choose **URL** option in Import dialog window to enter the link
<img src="documentation/assets/workspace-import-dialog.png" alt="Workspace Import Dialog" width="500"/>

**Notebooks Execution:**
* You will get the following notebooks in **td-profiler** workspace folder after import.
![td-profiler folder](documentation/assets/td-profiler-nootebook-folder.png)
<br>
* Run the Notebooks is the following sequece.
<br>

  **1 td_profiler_usage_data_extract_queries** :
  * No need to run this notebook directly. 
  * This notebook includes the SQL queries for usage data extract and it is used  by  `td_profiler_usage_data_extract` notebook.
  * You can review these queries used by profiler for usage data extract.
  * if required You can also ajust the duration in number of days for the various data extract queries to control the volume of the data.
        <img src="documentation/assets/PDCR_query_hist_SQL_sample.png" alt="Sample Usage Data Extract Query" width="400"/>
<br>

  **2) td_profiler_usage_data_extract.scala** :
  * It is first notebook you need run.
  * Attach the notebook to cluster created above with required configuration
  * Run the `Cmd 2` cell with following content to create Notebook widgets to enter the JDBC connection details
    ```
      dbutils.widgets.text("dbHost", "<SetThis>")
      dbutils.widgets.text("dbUser", "<SetThis>")
      dbutils.widgets.text("dbpwd", "<SetThis>")
      dbutils.widgets.text("targetSchema", "migrations_td_profiler")
    ```
  * Enter Notebook Input Parameters values
    <img src="documentation/assets/data_extract_notebook_widgets.png" alt="Usage Data Extract Notebook Widgets" width="800"/>
  * :information_source: `dbpwd` notebook widget will be removed after the execution of susequent code cells in the notebook. If you need to re-enter the db password, rerun the `Cmd 2` again to recreate `dbpwd` notebook widget.
  * You can either Run rest of the code cells in the Notebook individually or Run the entire notebook by selecting the "Run All" option from Notebook Menu.
  * Execution time of this Notebook depends on the volume of your usage data.
  * It is also recomended to run the profiler notebooks during off-peek hours.
<br>

  **3) td_profiler_usage_data_analysis.sql** :
  * Run this Notebook after the successfull execution of `td_profiler_usage_data_extract`
  * Export this notebook with execution results as `DBC Archive` format to share with Databricks team.

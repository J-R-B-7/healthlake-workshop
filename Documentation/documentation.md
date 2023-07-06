# AWS HEALTHLAKE

[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/ashish493/alphanumeric_recognition/issues)

**Problem Statement**

For health data, it's common for it to be incomplete and inconsistent. Some examples of unstructured data(for example, heart ECG or brain EEG traces) are Clinical notes, lab reports, insurance claims, medical photographs, taped conversations, and time series. This information is intended for humans to grasp, but not for machines to comprehend.The following are the major obstacles to using healthcare data:
- How to gain a complete picture of the data by combining organized and
unstructured data?
- How do we evaluate the prediction findings intuitively?
- How do we automate digital transformation in healthcare?

****

**An overview of AWS Healthlake**

Amazon Healthlake is a HIPAA-eligible service that enables healthcare and life sciences companies to securely store data, transform it into a consistent, queryable format, and further analyze that data in the cloud on a petabyte-scale. The HealthLake API allows healthcare institutions to easily copy , such as medical image reports and patient notes,from on-premises systems to a secure data lake in the cloud.

****

**Pre-Requisites**
- AWS Management Console : It acts as a web interface that we can use to access HealthLake.
- AWS Command Line Interface (AWS CLI) : This helps in providing commands for a broad set of AWS services, including HealthLake, and is supported on Windows, macOS and Linux.
- AWS SDKs : Amazon Web Services provides SDKs (software development kits) that comprises of different libraries and sample code for various programming languages. Ultimately, it provides a convenient way to access HealthLake.
- Data Visualization Tools: Amazon Quicksight is used for building various business insights and predictions.

****

**Workflow**

Amazon Healthlake is broadly divided into four sections:
1. In the first section, a SageMaker notebook was created and a dummy patient population was generated using Synthea. Then, a HealthLake Data Store was created, the generated data was imported using Synthea and then it was exported to an S3 bucket.
2. In the second section, the exported data was transformed using Glue to extract the information added by HealthLake. For example, the Glue Job extracts the JSON Extension field and puts it in a separate table which can be used for building the dashboard.
3. In the third section, Athena was used for joining the individual tables into the required higher-level combined tables.
4. In the fourth section, Datasets were created in Quicksight and the QuickSight dashboard was built.

****

**Implementation**

1. HealthLake Setup, import and export
- This mainly includes the creation of Sagemaker Instance for workshop use.
- After a successful deployment of CloudFormation stack with necessary resources,
S3DataBucket and KMSImportExportLogsKey values were noted for further use.
- Then, we need to focus on generating sample data set from the Sagemaker notebook.
- Once the synthetic data is created, it was uploaded to theS3 bucket for further steps.
- The next step includes the creation of a HealthLake data store. I navigated to the Amazon HealthLake console and created HealthLake Data Store.
- After the successful creation of the "Healthlake-workshop" Datastore, its ID was noted. Hence, the data is imported to HealthLake and its < job_id > was noted.
- Similarly, the data is exported from Healthlake mentioning its ARNs in KMS policy and its < job_id > was noted.

2. Data Transformation using Glue
- This mainly includes how the data exported from Amazon Healthlake was used and tables were created in Athena for ad-hoc querying. DocumentReference resource parser script was uploaded to the S3 bucket by running the required command in the JupyterLab terminal.
- After the successful deployment of the healthlake-workshop-glue stack, I navigated to the AWS Glue console and moved to the trigger section."healthlake-trigger" was started and the workflow section was observed until it was completed.
- The next step includes the running of Crawler from the DocRefCrawler. Once, the Run Crawler finished successfully, a new table named parseddocref was created in healthlakedb.

3. Table preparation using Athena
- This mainly includes how the tables created using Glue were used for making Dashboard. The tables created by Glue Job crawled the HealthLake output and created
tables which were queried via Athena. Then, for the meaningful analysis, I joined the tables together and created dashboards in Athena.
- I have created two tables as per the following queries in Athena using healthlakedb:
- condition_patient_encounter table was created by joining the Condition, Patient and Encounter tables.
- parseddocref_patient_encounter table was created by joining the parseddocref, Patient and Encounter tables.

4. Creating Datasets
- This mainly includes how the created Datasets will power the QuickSight Analyses. I navigated to QuickSight and logged in to it and used the standard version for the
required task. Then, one permission was needed for Athena Query Results from S3 Bucket, for which, I set up the Athena as a data source in QuickSight.
- After getting the QuickSight console access successfully, New DataSet was created from "Create a DataSet From New Data Sources" on Athena.
- There was some Data type correction needed for the start_enc column. So, that specific column was selected and yyyy-MM-dd'T'HH:mm:ssZ was written in the box of Data type, validated and finally updated the column type to Date.

5. Building QuickSight Dashboards

This section mainly focused on creating two tabs in the workshop:
- The first tab contains the graphs and charts on the patient population.
- The second tab contains the patient timeline comparing the conditions from
structured and unstructured data.

****

5. Building QuickSight Dashboards

This section mainly focused on creating two tabs in the workshop:
- The first tab contains the graphs and charts on the patient population.
	- The first widget was created to count the total number of patients.
	- The second widget was created to present the Patient Distribution by Gender.
	- The third widget was created to show the Distribution by Encounter Start Date, which also demonstrates the usage of built-in "Anomaly insight" (or "forecast insight").
	- The last widget was created for the Distribution of Encounters by Conditions.

- The second tab contains the patient timeline comparing the conditions from
structured and unstructured data.
	- The first table generated the patient timeline using the conditions from structured data.
	- The second table generated the timeline using conditions parsed from
unstructured data using Amazon HealthLake.

****

**Benefits of AWS HealthLake**

With the help of Amazon Healthlake, you can:
- Rapidly and effortlessly capture health data: A service (Amazon S3) bucket
that allows you to bulk import local Fast Healthcare Interoperability Resources
(FHIR) files into Amazon Simple Storage, including clinical notes, claims, lab
reports, and more. You can then use the data in downstream applications or
workflows.
- Get easily queriable data: HealthLake creates an absolute chronological view
of each patient's medical history and configures it in the R4 FHIR standard
format.
- Transform unstructured data using a special ML model: Embedded Medical
Natural Language Processing (NLP) is a special ML model trained to understand
and extract meaningful information from unstructured health data. Use to convert
raw medical text data. With integrated medical NLP, entities (such as medical
procedures and drugs), entity relationships (such as drugs and their doses), and
entity characteristics (test results positive or negative, procedure time, etc.) are
automatically determined. It can extract, data from our medical text.
- Use powerful query and search capabilities: HealthLake supports Fas-
Healthcare Interoperability Resources CRUD (create/read/update/delete) and
FHIR search.

****

**Applications**

Amazon HealthLake can be used for the following healthcare applications:
- Population Health Management:
HealthLake uses advanced analytical tools and ML models to help healthcare
organizations analyze the health trends, results, and costs of the population. This
supports the organization to pinpoint the most appropriate interventions for the
patient population and select the more appropriate care management options.
- Improving the quality of care:
HealthLake provides a complete view of a patient's medical history so that
hospitals, medical planning, and life sciences organizations can fill gaps in care,
polish the quality of care, and reduce costs. In addition, it uses a complete
timeline view of the patient's medical history to bridge care gaps and identify
opportunities to apply prophylactic treatment.
- Optimize Hospital Efficiency:
HealthLake provides hospitals with key analytical and ML tools to improve and
polish up the efficiency and reduction of hospital waste. In most cases, advanced
analysis and ML are applied to the reconstructed data to optimize scheduling,
reduce unnecessary steps, and predict bed availability.

****

**Conclusion**

With the help of Machine Learning, organizations have been able to automate repetitive
manual processes leading to an increase in operational efficiency. Interactive dashboards
showcasing patient view and claims history were made for effective visualization, which
demonstrated how organizations can leverage the power of Machine Learning to achieve
speed and agility. The timeframe for certain significant analyses has been reduced to
days and hours as opposed to months, with the help of AWS tools like Amazon
HealthLake, Amazon SageMaker, Athena, and QuickSight. Amazon HealthLake with
other AWS services can be utilised by any healthcare providers, payers, and
pharmaceutical companies that operate in US East (N. Virginia), US East (Ohio), and
US West (Oregon) Regions. To provide better patient care, Amazon HealthLake has
been used with FHIR Works. A compliant FHIR data store can be created with
integrated NLP and analytics. AWS has partnered with Cognosante, Innovaccer, Redox,
InterSystems, Diameter Health, and HealthLX to deliver innovative and transformative
solutions to federal, state, and local governments.

****

**Future Work**

Amazon HealthLake’s NLP capabilities can be used to automate health data processing
by extracting useful information from unstructured data including medical conditions,
medications, and procedures. Automated health data processing will reduce a load of
healthcare professionals by eliminating the time consumed for medical comprehension.
Deep learning models can be employed to make more accurate models without extensive
feature extraction and feature engineering. The need for chart abstraction for analyses
can be eliminated by replacing the same with automated data pipelines which derive
insights from physician notes as well.

Amazon HealthLake is entering a new era of data analytics. Soon, the need for data
engineers and technical staff will be minimum. The unknown events and happening can
be predicted by learning from historical data patterns, eventually.

****

**Acknowledgements**

 My project is based on [this](https://catalog.us-east-1.prod.workshops.aws/workshops/498fdbc5-46e1-4cb0-97a0-f4ec3a30f26a/en-US) workshop.

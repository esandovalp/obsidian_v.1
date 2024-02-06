#google 
## Designing Data Processing Systems forum

### Diagnostic Questions (1st try - 20%)

1. Laws in the region where you operate require that **files related to all orders made each day are stored immutably for 365 days.** The solution that you recommend has to be **cost-effective.** What should you do?
    1. My answer: ~~Store the data in a Cloud Storage bucket, and set a lifecycle policy to delete the file after 365 days.~~
        1. Why not?: Incorrect. This option automates the deletion of the file after 365 days, but **it does not restrict the deletion of the file before that**.
2. Business analysts in your team need to **run analysis on data that was loaded into BigQuery.** You need to follow recommended practices and grant permissions. What role should you grant the business analysts?
    1. My answer: ~~bigquery.resourceViewer and bigquery.dataViewer~~
        1. Why not? Incorrect. **The resourceViewer role grants permissions to view capacity and reservations, which are not sufficient to conduct analysis.**
3. You are using **Dataproc to process a large number of CSV files**. The storage option you choose needs to be flexible to serve many worker nodes in multiple clusters. These worker nodes will read the data and also write to it for intermediate storage between processing jobs. What is the recommended storage option on Google Cloud?
    1. My answer: Cloud Storage
        1. Why?: Correct. **Cloud Storage is the recommended, centralized storage option for Dataproc**. It offers many benefits such as high data availability, no storage management, quick startup, and consistent Identity and Access Management (IAM).
4. You are managing the data for Cymbal Retail, which consists of multiple teams including retail, sales, marketing, and legal. These teams are consuming data from multiple producers including point of sales systems, industry data, orders, and more. Currently, teams that consume data have to repeatedly ask the teams that produce it to verify the most up-to-date data and to clarify other questions about the data, such as source and ownership. This process is unreliable and time-consuming and often leads to repeated escalations. You need to **implement a centralized solution that gains a unified view of the organization's data and improves searchability**. What should you do?
    1. My answer: ~~Implement a data warehouse by using BigQuery, and create datasets for each team such as retail, sales, marketing.~~
        1. Why not?: Incorrect. **BigQuery can separate the data in datasets, but it is not a centralized data discovery solution that can hold different types of data.**
5. Cymbal Retail is migrating its private data centers to Google Cloud. Over many years, hundreds of terabytes of data were accumulated. You currently have a 100 Mbps line and y**ou need to transfer this data reliably before commencing operations on Google Cloud** in 45 days. What should you do?
    1. My answer: ~~Zip and upload the data to Cloud Storage buckets by using the Google Cloud console.~~
        1. Why not?: Incorrect. **Uploading large amounts of data through the Google Cloud console is not convenient or performant.**
6. You are migrating on-premises data to a data warehouse on Google Cloud. This data will be made available to business analysts. Local regulations require that customer information including credit card numbers, phone numbers, and email IDs be captured, but not used in analysis. You need to use a reliable, recommended solution to redact the sensitive data. What should you do?
    1. My answer: ~~Use the Cloud Data Loss Prevention (DLP) API to perform date shifting of any entries with credit card numbers, phone numbers, and email IDs.~~
        1. Why not?: **Date shifting the entries does not redact the personal identifiable information (PII) like credit card numbers, phone numbers, and email IDs.**
7. Cymbal Retail has a team of business analysts who need to fix and enhance a set of large input data files. For example, duplicates need to be removed, erroneous rows should be deleted, and missing data should be added. These steps need to be performed on all the present set of files and any files received in the future in a repeatable, automated process. The business analysts are not adept at programming. What should they do?
    1. My answer: Load the data into Dataprep, explore the data, and edit the transformations as needed.
        1. Why?: Correct. **Dataprep lets you load large amounts of data and visually fix it, which would be very convenient for those who are unfamiliar with programming**. The data wrangling steps can be captured as a series of transformations that can be reapplied later to future data.
8. You have a Dataflow pipeline that runs data processing jobs. You need to **identify the parts of the pipeline code that consume the most resources.** What should you do?
    1. My answer: ~~Use Cloud Monitoring~~
        1. Why not?: Incorrect. Cloud Monitoring provides useful diagnostics about Dataflow jobs, but for this requirement, you need the features in **Cloud Profiler**.
9. Cymbal Retail has acquired another company in Europe. Data access permissions and policies in this new region differ from those in Cymbal Retail’s headquarters, which is in North America. You need to define a consistent set of policies for projects in each region that follow recommended practices. What should you do?
    1. My answer: ~~Implement policies at the resource level that comply with regional laws.~~
        1. Why not?: Incorrect. **Applying policies at the resource level is time-consuming and might be inconsistent.**
10. Your data and applications reside in multiple geographies on Google Cloud. Some regional laws require you to hold your own keys outside of the cloud provider environment, whereas other laws are less restrictive and allow storing keys with the same provider who stores the data. The management of these keys has increased in complexity, and you need a solution that can centrally manage all your keys. What should you do?
    1. My answer: ~~Store keys in Cloud Key Management Service (KMS), and reduce the number of days forautomatic key rotation.~~
        1. Why not? Incorrect. **Cloud KMS stores keys on Google Cloud; therefore, this won't meet the requirement for the data and the keys that will be stored in different provider environments.**

### Diagnostic Questions (2nd try - 60%)

1. Laws in the region where you operate require that **files related to all orders made each day are stored immutably for 365 days.** The solution that you recommend has to be **cost-effective.** What should you do?
    1. My answer: ~~Store the data in a Cloud Storage bucket, and set a lifecycle policy to delete the file after 365 days.~~
        1. Why not?: Incorrect. This option automates the deletion of the file after 365 days, but **it does not restrict the deletion of the file before that**.
2. Business analysts in your team need to **run analysis on data that was loaded into BigQuery.** You need to follow recommended practices and grant permissions. What role should you grant the business analysts?
    1. My answer: ~~bigquery.resourceViewer and bigquery.dataViewer~~
        1. Why not? Incorrect. **The resourceViewer role grants permissions to view capacity and reservations, which are not sufficient to conduct analysis.**
3. You are using **Dataproc to process a large number of CSV files**. The storage option you choose needs to be flexible to serve many worker nodes in multiple clusters. These worker nodes will read the data and also write to it for intermediate storage between processing jobs. What is the recommended storage option on Google Cloud?
    1. My answer: Cloud Storage
        1. Why?: Correct. **Cloud Storage is the recommended, centralized storage option for Dataproc**. It offers many benefits such as high data availability, no storage management, quick startup, and consistent Identity and Access Management (IAM).
4. You are managing the data for Cymbal Retail, which consists of multiple teams including retail, sales, marketing, and legal. These teams are consuming data from multiple producers including point of sales systems, industry data, orders, and more. Currently, teams that consume data have to repeatedly ask the teams that produce it to verify the most up-to-date data and to clarify other questions about the data, such as source and ownership. This process is unreliable and time-consuming and often leads to repeated escalations. You need to **implement a centralized solution that gains a unified view of the organization's data and improves searchability**. What should you do?
    1. My answer: ~~Implement a data warehouse by using BigQuery, and create datasets for each team such as retail, sales, marketing.~~
        1. Why not?: Incorrect. **BigQuery can separate the data in datasets, but it is not a centralized data discovery solution that can hold different types of data.**
5. Cymbal Retail is migrating its private data centers to Google Cloud. Over many years, hundreds of terabytes of data were accumulated. You currently have a 100 Mbps line and y**ou need to transfer this data reliably before commencing operations on Google Cloud** in 45 days. What should you do?
    1. My answer: ~~Zip and upload the data to Cloud Storage buckets by using the Google Cloud console.~~
        1. Why not?: Incorrect. **Uploading large amounts of data through the Google Cloud console is not convenient or performant.**
6. You are migrating on-premises data to a data warehouse on Google Cloud. This data will be made available to business analysts. Local regulations require that customer information including credit card numbers, phone numbers, and email IDs be captured, but not used in analysis. You need to use a reliable, recommended solution to redact the sensitive data. What should you do?
    1. My answer: ~~Use the Cloud Data Loss Prevention (DLP) API to perform date shifting of any entries with credit card numbers, phone numbers, and email IDs.~~
        1. Why not?: **Date shifting the entries does not redact the personal identifiable information (PII) like credit card numbers, phone numbers, and email IDs.**
7. Cymbal Retail has a team of business analysts who need to fix and enhance a set of large input data files. For example, duplicates need to be removed, erroneous rows should be deleted, and missing data should be added. These steps need to be performed on all the present set of files and any files received in the future in a repeatable, automated process. The business analysts are not adept at programming. What should they do?
    1. My answer: Load the data into Dataprep, explore the data, and edit the transformations as needed.
        1. Why?: Correct. **Dataprep lets you load large amounts of data and visually fix it, which would be very convenient for those who are unfamiliar with programming**. The data wrangling steps can be captured as a series of transformations that can be reapplied later to future data.
8. You have a Dataflow pipeline that runs data processing jobs. You need to **identify the parts of the pipeline code that consume the most resources.** What should you do?
    1. My answer: ~~Use Cloud Monitoring~~
        1. Why not?: Incorrect. Cloud Monitoring provides useful diagnostics about Dataflow jobs, but for this requirement, you need the features in **Cloud Profiler**.
9. Cymbal Retail has acquired another company in Europe. Data access permissions and policies in this new region differ from those in Cymbal Retail’s headquarters, which is in North America. You need to define a consistent set of policies for projects in each region that follow recommended practices. What should you do?
    1. My answer: ~~Implement policies at the resource level that comply with regional laws.~~
        1. Why not?: Incorrect. **Applying policies at the resource level is time-consuming and might be inconsistent.**
10. Your data and applications reside in multiple geographies on Google Cloud. Some regional laws require you to hold your own keys outside of the cloud provider environment, whereas other laws are less restrictive and allow storing keys with the same provider who stores the data. The management of these keys has increased in complexity, and you need a solution that can centrally manage all your keys. What should you do?
    1. My answer: ~~Store keys in Cloud Key Management Service (KMS), and reduce the number of days forautomatic key rotation.~~
        1. Why not? Incorrect. **Cloud KMS stores keys on Google Cloud; therefore, this won't meet the requirement for the data and the keys that will be stored in different provider environments.**

### Diagnostic Questions (2nd try - 60%)


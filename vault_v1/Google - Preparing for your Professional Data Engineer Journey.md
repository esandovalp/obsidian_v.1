#google 
# Designing Data Processing Systems forum

## Diagnostic Questions (1st try - 50%, 2nd try - 90%)

1. You have a [[Dataflow pipeline]] that runs data processing jobs. You need to identify the parts of the pipeline code that consume the most resources. What should you do?
	1. Use [[Cloud Profiler]]
		1. Correct. Cloud Profiler shows you a flame graph of statistics of the running jobs, which can be used to evaluate resource usage.
2. Laws in the region where you operate require that files related to all orders made each day are stored immutably for 365 days. The solution that you recommend has to be cost-effective. What should you do?
	1. Store the data in a [[Cloud Storage & buckets]], and specify a retention period.
		1. Correct. Object retention is a built-in option that lets you configure how long the files should remain without allowing any changes.
3. Cymbal Retail has acquired another company in Europe. Data access permissions and policies in this new region differ from those in Cymbal Retail’s headquarters, which is in North America. You need to **define a consistent set of policies for projects in each region that follow recommended practices**. What should you do?
	1. Implement a flat hierarchy, and assign policies to each project according to its region.
		1. Incorrect. A flat hierarchy is not recommended, because policy settings need to be individually applied for each project.
	2. Create top level folders for each region, and assign policies at the folder level.
		1. Correct. **Folders are used to group related projects within an organization.** They can also be used to apply consistent policies for projects within the folder.
4. Business analysts in your team need to run analysis on data that was loaded into [[BigQuery]]. You need to follow recommended practices and grant permissions. What role should you grant the business analysts?
	1. bigquery.user and bigquery.dataViewer
		1. Correct. The analysts need to view the data and run queries on it, which are granted by these predefined roles.
5. You are migrating on-premises data to a [[Data warehouse]] on Google Cloud. This data will be made available to business analysts. Local regulations require that customer information including credit card numbers, phone numbers, and email IDs be captured, but not used in analysis. You need to use a reliable, recommended solution to redact the sensitive data. What should you do?
	1. Create a regular expression to identify and delete patterns that resemble credit card numbers, phone numbers, and email IDs.
		1. Incorrect. Determining the regular expressions that match different data types can be more tedious and less accurate than using Cloud DLP.
	2. Use the Cloud Data Loss Prevention (DLP) API to identify and redact data that matches infoTypes like credit card numbers, phone numbers, and email IDs.
		1. Correct. [[Cloud Data Loss Prevention]] helps you discover, classify, and protect your most sensitive data. There are predefined infoTypes that you can employ to identify and redact specific data types.
6. Cymbal Retail has a team of business analysts who need to fix and enhance a set of large input data files. For example, duplicates need to be removed, erroneous rows should be deleted, and missing data should be added. These steps need to be performed on all the present set of files and any files received in the future in a repeatable, automated process. The business analysts are not adept at programming. What should they do?
	1. Load the data into [[Dataprep]], explore the data, and edit the transformations as needed.
		1. Correct. [[Dataprep]] lets you load large amounts of data and visually fix it, which would be very convenient for those who are unfamiliar with programming. The data wrangling steps can be captured as a series of transformations that can be reapplied later to future data.
7. Cymbal Retail is migrating its private data centers to Google Cloud. Over many years, hundreds of terabytes of data were accumulated. You currently have a 100 Mbps line and you need to transfer this data reliably before commencing operations on Google Cloud in 45 days. What should you do?
	1. Upload the data to Cloud Storage by using gsutil.
		1. Incorrect. gsutil is useful for a few files of small to medium sizes. For larger data, other data transfer options should be considered.
	2. Order a [[Transfer appliance]], export the data to it, and ship it to Google.
		1. Correct. For large amounts of data that need to be transferred within a month, a transfer appliance is the right choice.
8. You are managing the data for Cymbal Retail, which consists of multiple teams including retail, sales, marketing, and legal. These teams are consuming data from multiple producers including point of sales systems, industry data, orders, and more. Currently, teams that consume data have to repeatedly ask the teams that produce it to verify the most up-to-date data and to clarify other questions about the data, such as source and ownership. This process is unreliable and time-consuming and often leads to repeated escalations. You need to **implement a centralized solution that gains a unified view of the organization's data and improves searchability**. What should you do?
	1. Implement Looker dashboards that provide views of the data that meet each teams’ requirements.
		1. Incorrect. Looker provides visualizations of the data based on the data itself. It does not have inherent data tagging facilities.
	2. Implement a data mesh with [[Dataplex]] and have producers tag data when created.
		1. Correct. [[Dataplex]] is a data mesh that also includes data cataloging capability with Data Catalog. Consumers of data can search and discover information readily without having to wait for data producers to respond, which reduces the bottlenecks on data analysis.
9. Your data and applications reside in multiple geographies on Google Cloud. Some regional laws require you to hold your own keys outside of the cloud provider environment, whereas other laws are less restrictive and allow storing keys with the same provider who stores the data. The management of these keys has increased in complexity, and you need a solution that can centrally manage all your keys. What should you do?
	1. Store your keys in Cloud Hardware Security Module (HSM), and retrieve keys from it when required.
		1. Incorrect. Cloud HMS is a Google Cloud service that stores keys on hardware security modules on Google Cloud. Hence, this won't meet the requirement for the data and the keys that will be stored in different provider environments.
	2. Enable confidential computing for all your virtual machines.
		1. Incorrect. Confidential computing does not affect the storage of data.
10. You are using Dataproc to process a large number of CSV files. The storage option you choose needs to be flexible to serve many worker nodes in multiple clusters. These worker nodes will read the data and also write to it for intermediate storage between processing jobs. What is the recommended storage option on Google Cloud?
	1. Cloud Storage
		1. Correct. Cloud Storage is the recommended, centralized storage option for Dataproc. It offers many benefits such as high data availability, no storage management, quick startup, and consistent Identity and Access Management (IAM).Correct. Cloud Storage is the recommended, centralized storage option for Dataproc. It offers many benefits such as high data availability, no storage management, quick startup, and consistent Identity and Access Management (IAM).
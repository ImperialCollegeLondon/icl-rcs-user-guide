# RDF-Active

The Research Data Facility - Active (or RDF-Active) is a large-scale storage service for research data in active use. The RDF-Active is one of several storage facilities (including the [RDF-HPC]()) that replaces the previous [Research Data Store](../rds/index.md) service.

You can access the RDF-Active as a Windows Network Share (SMB) from Windows, Linux, or macOS -- either on campus or remotely via the University's [Remote Access](../remoteaccess.md) service. Access via Globus will be available soon. The RDF-Active runs on IBM's high-performance, scalable Storage Scale file system (on Lenovo hardware) and stores all data at two separate sites more than 30 km apart for resilience.

/// details | Can my data be stored on the RDF-Active? (data security)
    type: faq
As with the RDS, the RDF-Active is only suitable for storing data which is either public data or unrestricted data as per the [glossary definitions](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/#glossary) of data classification at Imperial. You will find recommendations on where to store other data classifications on the wider [Saving My Files](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/) page.

We strongly recommend you contact the [data protection coordinator](https://www.imperial.ac.uk/admin-services/governance/policies-and-guidance/contact-us/) for your department or faculty if you have any concerns or questions relating to the classification of your data.
///

## Storage Allocations

On the RDF-Active, storage is provisioned through storage allocations. Each allocation comes with a maximum storage quota, an access group code as well as a research group to own that allocation. Research groups can have multiple storage allocations assigned to them at any given time.

You can organise your storage allocations to fit your workflow. Some groups prefer a single large allocation that hosts multiple projects, while others create separate allocations for each research project. 
XXX So is it one allocation split into smaller partitions or what? Or both
Whichever method you decide to use to organise your data, we strongly recommend you follow the [best practices for managing your research data](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/) available online.

## How to Apply for Access

/// details | How can I request a new RDF-Active Storage Allocation?
    type: faq
Information on how to request a new RDF-Active storage allocation will appear here soon. 
XXX will be filled in later
///

/// details | What is the process to request migrating my RDS projects to RDF-Active?
    type: faq
We have a [dedicate page](./rds-rdfactive-migration.md) to provide information and advice on migrating from the RDS to the RDF-Active.
///

/// details | How do I request access to an existing storage allocation?
    type: faq
Please contact the research group lead of the storage allocation, or its designated administrator, who can provide you access. 
XXX how is this found?
///

## How to Access the RDF-Active

Please view our dedicated section on [Accessing the RDF-Active](./access/index.md).

## Cost

The RDF-Active is charged on a *per TB of quota* basis, prepaid for a mininum term of 12 months; all data is stored with two copies (i.e. dual copy) in physically separate locations. Current rates may be found on the [RCS Charging Structure](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/) page.

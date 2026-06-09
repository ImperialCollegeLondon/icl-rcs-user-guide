# RDF-Active
TODO rename file to `rdfactive-index.md`

The "Research Data Facility - Active" (or the RDF-Active) is a large-scale storage service for research data in active use. The RDF-Active is one of several storage facilities that replaces the previous [Research Data Store](../rds/index.md) service. You can access the RDF-Active as a Windows Network Share (SMB) from Windows, Linux, or macOS -- either on campus or remotely via the University's [Remote Access](../remoteaccess.md) service. Access via Globus will be available soon.

The RDF-Active runs on IBM's high-performance, scalable Storage Scale file system with Lenovo hardware and stores all data at two separate sites more than 30 km apart for resilience.

/// details | Can my data be stored on the RDF-Active? (data security)
    type: faq
As with the RDS, the RDF-Active is only suitable for storing data which is either public data or unrestricted data as per the [glossary definitions](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/#glossary) of data classification at Imperial. You will find recommendations on where to store other data classifications on the wider [Saving My Files](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/) page.

We strongly recommend you contact the [data protection coordinator](https://www.imperial.ac.uk/admin-services/governance/policies-and-guidance/contact-us/) for your department or faculty if you have any concerns or questions relating to the classification of your data.
///

## Storage Allocations

On the RDF-Active, storage is provisioned through storage allocations. Each allocation comes with a maximum storage quota, an access group code, and a research group assigned as the owner of that allocation. Research groups can have multiple storage allocations assigned to them at any given time.

You can organise your storage allocations to fit your workflow. Some groups prefer a single large allocation that hosts multiple projects, while others create separate allocations for each research project.

Whichever method you decide to use to organise your data, we strongly recommend you follow the [best practices for managing your research data](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/) available online.

## How to Apply for Access

/// details | How can I request a new RDF-Active storage allocation?
    type: faq
Due to overhead issues outside of the RCS department's control, requesting a new storage allocation is currently done indirectly by using the [RDS to RDF-Active migration ticket request](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2) system. In order to submit a migration form in general, a file must be attached where normally the RDS projects to be migrated are specified. To make it clear the migration form is actually a request for a new storage allocation, attach a text file that instead asks for a new and *empty* storage allocation whilst skipping any other parts on the form discussing the RDS.

We apologise for any confusion and inconvenience.
///

/// details | How do I request access to an existing storage allocation?
    type: faq
To gain access to an allocation, you must be a part of the research group associated with said allocation. Contact the research group lead of the storage allocation or its designated administrator, who can provide you access.
///

/// details | How do I manage a storage allocation?
    type: faq
Most storage allocations don't need much managing after initialisation outside of [access control](../recap/allocations.md#user-management). However, if you want to change the storage quote or file quota of a storage allocation, then [create a ticket](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/) with us.
///

/// details | What is the process to request migrating my RDS projects to RDF-Active?
    type: faq
We have a [dedicated page](./rds-rdfactive-migration.md) to provide information and advice on migrating from the RDS to the RDF-Active.
///

## How to Access the RDF-Active

Please view our dedicated section on [Accessing the RDF-Active](./access/index.md).

## Cost

The RDF-Active is charged on a *per TB of quota* basis that's prepaid for at least a 12 month term. All data is stored at two physically separate locations for redundacy (i.e. there are two separate but identical copies of your data at all times). Current cost rates may be found on the [RCS Charging Structure](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/) page.

## How Does the RDF-Active Differ from the RDS?

You can see [key differences](./rds-rdfactive-migration.md#key-differences-between-the-rds-and-the-rdf-active) between the two services on our userdoc page for migrating between them.

# RDF-Active

The Research Data Facility - Active (or RDF-Active) is a large storage service provided by the RCS for active or working research data. The RDF-Active is one of several storage facilities (including the RDF-HPC) that replaces the single storage service provided by the [Research Data Store or RDS](../rds/index.md).

The RDF-Active runs on IBM's high-performance, scalable Spectrum Scale file system (on Lenovo hardware) and stores all data at two separate sites more than 30 km apart for resilience. You can access it as a Windows Network Share (SMB) from Windows, Linux, or macOS -- either on campus or remotely via the University's [Remote Access](../remoteaccess.md) service. Access via Globus will be available soon.

/// details | Can my data be stored on the RDF-Active? (data security)
    type: faq
As with the [RDS](../rds/index.md), the RDF-Active is only suitable for storing data which is either public or unrestricted as per [Imperial data classifications](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/#glossary). You will find recommendations on where to store different classifications of data on the [Saving My Files page](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/).

We strongly recommend you contact your [faculty or departmental data protection coordinator](https://www.imperial.ac.uk/admin-services/governance/policies-and-guidance/contact-us/) if you have any concerns or questions relating to the classification of your data.
///

## Storage Allocations

On the RDF-Active, storage is provisioned through storage allocations. Each allocation has its own quota and access group and is owned by a research group. A research group can have one or multiple allocations.

You can organise storage allocations to fit your workflow. Some groups prefer a single large allocation that hosts multiple projects, while others create separate allocations for each research project. Whichever method you decide to use to organise your data, we strongly recommend you follow [best practice for managing your research data](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/).

## How to Apply for Access

/// details | How can I request a new RDF-Active Storage Allocation?
    type: faq
Information on how to request a new RDF-Active storage allocation will appear here soon.
///

/// details | What is the process to request migrating my RDS projects to RDF-Active?
    type: faq
We have begun contacting [RDS Project](../rds/index.md) owners to provide them a list of the projects for which they own so they can decide which of them to migrate to the RDF-Active (and in to how many storage allocations) and which projects can be closed down. For each storage allocation required on the RDF-Active, research group leads can then raise our form to raise a [Migration of RDS Project Space to RDF Group Space request](https://servicemgt.imperial.ac.uk/esc?id=sc_category&sys_id=52a4a8f21be62110557837b5464bcbd2&catalog_id=-1&spa=1).

Eventually all RDS project owners will be contacted but if you are yet to have been contacted and are interested in migrating sooner, please [raise a ticket with us](../support/index.md) so we can provide you the necessary information to complete the migration form.
///

## How to Access the RDF-Active

Please view our dedicated section on [Accessing the RDF-Active](./access/index.md).
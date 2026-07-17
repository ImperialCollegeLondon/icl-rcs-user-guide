# RDF-Active

The Research Data Facility - Active (RDF-Active) is a large-scale storage service designed for research data that is actively being used. It is one of several storage services introduced to replace the former [Research Data Store (RDS)](../rds/index.md).

RDF-Active can be accessed as a Windows network share (SMB) from Windows, Linux, and macOS devices, both on campus and remotely through the University's [Remote Access](../remoteaccess.md) service. Access is also available via Globus.

To provide resilience and protect against data loss, all data stored in RDF-Active is replicated across two separate sites located more than 30 km apart.

/// details | Can my data be stored on the RDF-Active? (data security)
    type: faq
As with the RDS, RDF-Active is only intended for data classified as **Public** or **Unrestricted** under Imperial's data classification scheme. For definitions of these classifications, please refer to the [Data Classification Glossary](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/#glossary). Guidance on appropriate storage locations for other data classifications can be found on the [Saving My Files](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/) page.

We strongly recommend you contact the [data protection coordinator](https://www.imperial.ac.uk/admin-services/governance/policies-and-guidance/contact-us/) for your department or faculty if you have any concerns or questions regarding the classification of your data.
///

## Storage Allocations

On RDF-Active, storage is provided through **storage allocations**. Each allocation has a defined storage quota, file quota, short name, file path, and an associated **research group** that owns and manages it. A research group can have multiple storage allocations at any one time.

**Research groups and storage allocations are managed through the [ReCAP self-service portal](../recap/index.md)**, where authorised users can create and administer research groups, and manage access permissions.

Storage allocations can be organised in a way that best supports your research activities. Some research groups choose to use a single large allocation for multiple projects, while others prefer to create separate allocations for individual research projects.

Regardless of how you choose to organise your allocations, we strongly recommend following Imperial's best practices for research data management, which are available on the [Research Data Management](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/) website.

## How to Apply for Access

/// details | How can I request a new storage allocation.
    type: faq

To request a new storage allocation on RDF-Active, please see the [Requesting a New Storage Allocation page](./requesting-a-new-allocation.md) for step-by-step guidance.
///

/// details | How do I request access to an existing storage allocation?
    type: faq
To gain access to an allocation, you must be a part of the research group associated with said allocation. Contact the research group lead of the storage allocation or a designated administrator, who can provide you access.
///

/// details | How do I manage a storage allocation?
    type: faq
Most storage allocations don't need much managing after initialisation outside of [access control](../recap/allocations.md#user-management). However, if you want to change the storage quote or file quota of a storage allocation, then [contact us](../support/index.md).
///

/// details | What is the process to request migrating my RDS projects to RDF-Active?
    type: faq
We have a [dedicated page](./rds-rdfactive-migration.md) to provide information and advice on migrating from the RDS to the RDF-Active.
///

## How to Access the RDF-Active

Please view our dedicated section on [Accessing the RDF-Active](./access/index.md).

## How Does the RDF-Active Differ from the RDS?

You can see [key differences](./rds-rdfactive-migration.md#key-differences-between-the-rds-and-the-rdf-active) between the two services on our userdoc page for migrating between them.

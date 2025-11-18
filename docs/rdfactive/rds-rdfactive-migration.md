# Migrating from the RDS to the RDF-Active

This page provides RDS project owners with the information they need to migrate data from RDS to RDF-Active.

## Key differences between the RDS and the RDF-Active

As an RDS Project owner or user, you will likely be aware of the following:

* An RDS Project is created and managed via the existing self-service portal ([https://selfservice.rcs.imperial.ac.uk/](https://selfservice.rcs.imperial.ac.uk/)).
* An RDS Project has a quota and an access list, both of which are managed via the above self-service portal. 
* All RDS Projects are essentially in the same folder/directory on the RDS.
* RDS Projects are only dual-copy if requested when the project is created.
* RDS Projects are charged on a month basis based on usage.
* The RDS is accessible via the CX3 HPC service.


In moving to the RDF-Active, you will now find the following:

* The RDF-Active is managed via a new self-service portal called [RECAP](../recap/index.md).
* We have introduced the concept of *Research Groups*. A *Research Group* is an administrative unit, typically led by a single PI, which is used to manage access to RCS resources such as *Storage Allocations* and future HPC systems. The *Research Groups* are managed (such as adding and removing members) via the RECAP portal. An individual may be a member of one or more *Research Groups* in RECAP.
* A *Storage Allocation* is a single location (directory or folder) on the RDF-Active file system with its own quota and access group. A *Research Group* can have one or more *Storage Allocations*. An individual must be a member of the *Research Group* before they can be added to the access group of a *Storage Allocation*. A *Storage Allocation* does not have to be tied to a specific research project!
* The file system structure now matches Imperial's structure: `/Faculty/Department/Research Group/Storage Allocation/` (see [Where are my files](./access/index.md#where-are-my-files) for more details).
* All data on the RDF-Active is replicated to a second location by default (i.e. dual-copy). There is no additional charge for this service.
* RDF-Active storage allocations are pre-paid for a minimum of 12 months, based on a requested level of quota.
* The RDF-Active is not directly accessible via the HPC services - please see our [FAQ](./rdfactive-faq.md#why-isnt-the-rdf-active-accessible-from-hpc-systems) for more information.

## Reviewing your RDS Projects

We have begun contacting [RDS Project](../rds/index.md) owners to provide them a list of the RDS projects they own for them to review; if you have not yet received this spreadsheet and would prefer not to wait, please [raise a ticket with us](../support/index.md) so we can send you a copy. In reviewing the list of RDS Projects, you have two options for each one:

* **Deletion:** If you no longer need the RDS Project space and have taken any necessary steps to preserve what data you require from it then you can request for the project to be deleted by [raising a ticket with us](../support/index.md). *Please note that we can only take requests to close an RDS Project from the Project owner or designated administrator.*
* **Migration:** You can fill in our migration form to request one or more RDS projects to be migrated to the RDF-Active. Please see the [following section for more information](#what-to-consider-before-submitting-the-migration-form).

## What to consider before submitting the migration form

!!! warning
    The current [RDS to RDF-Active migration form](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2) contains several inaccuracies regarding terminology, namely where you see references to RDF Group Space, this should instead refer to and **RDF-Active Storage Allocation**. Note also that you are **NOT** limited to G and P codes as the form infers, you may also use other codes including departmental F codes.
    
    We are working to correct the form as soon as possible but please [contact us](../support/index.md) if you have any questions in relation to this.

### What should I name the research group?


The first question on the migration form is whether the migration is On behalf of myself or someone else. The option you provide here will be used to set up the *Research Group* in the self-service portal and *Research Group* path on the file system will be based on the username of the Research Group lead.

If your *Research Group* is led by multiple individuals and/or you would like a different "file system friendly" path, please get in [contact with us](../support/index.md) before you submit the migration form. 

### How many storage allocations do I need?


The RDS to RDF-Active migration form must be filled in once per Storage Allocation required on the RDF-Active (remember that a Storage Allocation has its own quota and access group).

#### I only have one RDS Project to migrate

If you only have one RDS Project to migrate to the RDF-Active, then you only need to fill the form in once, attaching a file containing the name of the RDS Project to migrate (this can either be the spreadsheet that we've provided you or simply a text file with the name of the RDS Project). When filling in the form, please make sure to request suffient quota to cover the volume of data currently stored on the RDS as well as any additional usage for the next 12 months after moving to the RDF-Active.

#### I have multiple RDS Projects to migrate

When you have multiple RDS Projects to migrate to the RDF-Active, you have essentially three options:

* **A one-to-one mapping of RDS Projects to RDF-Active storage allocations**. You may want every RDS Project to map to a single Storage Allocation on the RDF-Active if, for example, you need to very clearly separate out who can access which project. You will need to fill in the migration form for every RDS Project you wish to migrate to the RDF-Active, attaching either the spreadsheet we've provided with just the row containing the corresponding project, or a text file containing the name of the project.
* **A many-to-one mapping of multiple RDS Projects to a single RDF-Active storage alloction**. You may want to do this if you find it easier to have all of your RDS Projects copied into a single "shared" space for your research group. You will only need to fill the form in once but you will need to attach a file containing a list of the projects you wish to merge to the single storage allocation; please make sure to request a quota level that covers the data volume from all the RDS Projects you wish to migrate to the RDF-Active as well as any expected new usage for the next 12 months. When it comes to migrating your data, we will copy each RDS Project into a similarly named directory/folder within your "shared" storage allocation.
* **Merging some RDS projects but not others** As a combination of the previous two options, you may wish to merge some RDS Projects into a single storage alllocation on the RDF-Active but not others. To achieve this, follow the guidance in the previous two options, filling in the form once per storage allocation you'd like on the RDF-Active.

### What do I do now?

If you believe you have sufficient information to fill in the [RDS to RDF-Active migration form](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2), please go ahead and do so. Please [contact us](../support/index.md) if you have any further questions.

## What happens when you submit the migration form

1. **Initial Processing**: When your form is initially submitted, it will go direct to the ICT Demand and Recharge Team who will review your request and arrange for the transfer of funds from the account you specified.
1. **Allocation to a migration stage**: Once your ticket is passed to us we will allocate you to a "migration stage" which will be dependent upon when we receive the request and the type of data you have. We will/have begun transferring projects in "Stage 1" and given the large number of projects and the volume of data to migrate, it will likely take months for us to transfer all projects in this stage. Once we have finished transferring users in "Stage 1", we will start migrating users in "Stage 2". 
1. **Data Migration**: When we get to the stage of migrating your data, our team will:
    * Notify you that data migration is starting.
    * Create a "[Research Group](#what-research-group-to-migrate-the-data-to)" in the RECAP portal if one does not exist already.
    * Set up a storage allocation with the specified quota and access group.
1. **Data Transfer**: Once your storage allocation has been created, we will use our dedicated data transfer tool to begin copying your data from the RDS to the RDF-Active.
1. **Finalise Transfer**: After most of the data has been transferred, our team will:
    * Contact you to arrange a time for removing access from the RDS and completing the final transfer. Please make sure you contact the current users of your RDS Project so that they know to stop using the RDS at the agreed time.
1. **Start using the RDF-Active**: Once the transfer has completed you will be able to start using the RDF-Active. You will need to add anyone who needs to access a storage allocation, first to your research group (if they are not already a member) and then to the corresponding storage allocation. Any migrated RDS Project/s will be disabled in the old self-service portal and will no longer be accessible.

!!! note
    * The RDS project currently has 1700+ projects using approximately 9PB of storage, we are therefore expecting it to take many months to migrate RDS Projects to the RDF-Active.
    * We are running the RDS as a fully maintained and production facility until **all** necessary data is transferred to the RDF-Active.

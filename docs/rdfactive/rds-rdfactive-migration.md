# Migrating from the RDS to the RDF-Active

This page provides RDS project owners with the information they need to migrate data from the RDS system to the RDF-Active.

!!! warning
    The current [RDS to RDF-Active migration form](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2) contains several inaccuracies regarding terminology, namely where you see references to "RDF Group Space", this should instead refer to and "RDF-Active Storage Allocation". Note also that you are **NOT** limited to G and P codes as the form infers, you may also use other codes including departmental F codes.

    We are working to correct the form as soon as possible but please [contact us](../support/index.md) if you have any questions in relation to this.

!!! note
    * The RDS project currently has 1700+ projects using approximately 9PB of storage, therefore **we are expecting it to take many months to migrate all RDS Projects to the RDF-Active**.
    * We are running the RDS as a fully maintained and production facility until **all** necessary data is transferred to the RDF-Active.

## Key differences between the RDS and the RDF-Active

As an RDS Project owner or user, you will likely be aware of the following:

* An RDS Project is created and managed via its own [self-service portal](https://selfservice.rcs.imperial.ac.uk/).
* An RDS Project has a quota and an access list, both of which are managed via the above self-service portal. 
* All RDS Projects are essentially in the same folder/directory on the RDS.
* RDS Projects are only dual-copy if requested when the project is created.
* RDS Projects are charged on a month basis based on usage.
* The RDS is accessible via the [CX3 HPC service](../hpc/cluster-specification.md#cx3).


In moving to the RDF-Active, you will now find the following:

* The RDF-Active is managed via a new self-service portal called [ReCAP](../recap/recap-index.md) that will support multiple RCS services.
* We have introduced the concept of research groups. A research group is an administrative unit, typically led by a single PI, which is used to manage access to RCS resources such as storage allocations on the RDF-Active. Research groups are managed  via the ReCAP portal. An individual may be a member of one or more research groups in ReCAP.
* One storage allocation is a single location (directory or folder) on the RDF-Active file system with its own quota and access group. As such, the file system structure now follows a hierarchy mirroring that of Imperial's department structure: `/Faculty/Department/Research Group/Storage Allocation/`. View the documentation on [Where are my files?](./access/index.md#where-are-my-files) for more information.
* A research group can have one or more storage allocations. An individual must be a member of the research group before they can be added to the access group of a storage allocation, and permissions can be set so being a member of the research group doesn't mean access to all associated storage allocations. 
* A storage allocation does not have to be tied to a specific research project. It is simply an allocated storage space for the research group to do with as they professionally please.
* All data on the RDF-Active is replicated to a second location by default (i.e. dual-copy). There is no additional charge for this service.
* RDF-Active storage allocations are pre-paid for a minimum of 12 months, based on a requested level of quota. Usage is not a factor.
* The RDF-Active is not directly accessible via the HPC services - please see our [FAQ](./rdfactive-faq.md#why-isnt-the-rdf-active-accessible-from-hpc-systems) for more information.

Moreover, before a PI would often have had multiple RDS projects; each with their own access groups, billing, quotas, etcetera. But now with the RDF-Active, a PI has a research group that can have one or multiple allocations within.

## Reviewing your RDS Projects

We have begun contacting [RDS Project](../rds/index.md) owners to provide them a list of their RDS projects for them to review. If you have not yet received a spreadsheet from us and would like to ASAP, please [raise a ticket with us](../support/support-index.md) so we can send you a copy. For each of your RDS Projects, you have two options:

* **Deletion:** If you no longer need the RDS Project space and have taken any necessary steps to preserve what data you require from it then you can request for the project to be deleted by [raising a ticket with us](../support/support-index.md). Please note that we can only take requests to close an RDS Project from the Project owner or designated administrator.
* **Migration:** You can fill in our migration form to request one or more RDS projects to be migrated to the RDF-Active. Please check [What to consider before submitting the migration form](#what-to-consider-before-submitting-the-migration-form) for more information.

## What to consider before submitting the migration form

### What should I name the research group?

The first question on the migration form is whether the migration is "on behalf of myself or someone else". The option you provide here will be used to set up the research group in the self-service portal and the research group path on the file system will be based on the username of the research group's lead.

If your research group is led by multiple individuals and/or you would a more "file system friendly" path, please get in [contact with us](../support/support-index.md) before you submit the migration form.

### Are there ever cases where you need to submit multiple migration forms?

Yes. The form for migrating from the RDS to the RDF-Active must be filled in once for every storage allocation required on the RDF-Active. So the answer will depend on how you want to to manage who can and cannot access the allocation resources on the RDF-Active.

#### What do I do if I only have one RDS Project to migrate?

If you only have one RDS Project to migrate to the RDF-Active, then you likely only need to fill the form in once. When you submit a migration form, be sure to attach a file containing the name of the RDS Project to be migrated. This can either be the spreadsheet provided or simply a plain text file with the name of the RDS Project. When filling in the form, please make sure to request sufficient quota to cover the volume of data currently being stored on the RDS as well as any additional usage for the next 12 months after migrating to the RDF-Active.

#### What do I do if I only have one RDS Project to migrate?

When you have multiple RDS Projects to migrate to the RDF-Active, there are three core options:

* **A one-to-one mapping of RDS Projects to RDF-Active storage allocations**. This will be cut and dry migration with no change in who has access to the data and storage resources.
* **A many-to-one mapping of multiple RDS Projects to a single RDF-Active storage allocation**. You might find that it would be easier to combine all your projects into one storage allocation due to identical users between projects.
* **Merging some RDS projects but not others**. A combination of the previous two options carried out on a case-by-case basis as you fill in one form for each requested storage allocation.

Please [get in contact with us](../support/support-index.md) should you have any questions about this or possible alternatives.

### What do I do now?

If you believe you have sufficient information to fill in the [RDS to RDF-Active migration form](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2), please go ahead and do so. Please [contact us](../support/support-index.md) if you have any further questions.

## What happens when I submit the migration form?

1. **Initial Processing**: When your form is initially submitted, it will go direct to the ICT Demand and Recharge Team who will review your request and arrange for the transfer of funds from the account you specified.
2. **Data Migration**: Once your ticket is processed by ICT, we will verify the specified research group exists or create one for you. Next, we will begin setting up your requested storage allocation and notify you that your migration has begun. If the specified name of your access group is not available, we will contact you to discuss alternatives.
3. **Data Transfer**: With the storage allocation created and ready to be populated, we'll securely transfer the data over from the specified RDS project/s.
4. **Coordinate Switching Services**: When the data transfer is nearly complete, our team will contact you to arrange a time to officially remove your access from the RDS and finalise the transfer. Please make sure you coordinate with all the users on the relevant RDS Project/s so that they know to switch off from using the RDS at the agreed upon time.
5. **Set Up User Access Control**: Once the transfer has completed, any migrated RDS Project/s will be disabled on the old self-service portal and will no longer be accessible. Consequently, you will need to manage which users have access to the storage allocation. Only users in the associated research group can have access to the allocation, and you can add users to the research group as needed. Be sure to review our documentation on [access control](..\recap\research-groups.md#user-management) for research group managers. Much like with RDS, any user can be added to your research group as long as they have a full ICL. (See the ["Providing acces to RCS facilities (HPC, RDS) for visiting academics"](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) header for further information.)

## How will using the RDF-Active differ from using the RDS?

For most users who only used the RDS as a storage facility, very little will change. There shouldn't be any change in your typical workflow and it will be business as usual.

For HPC users, however, there is a change as the RDF facilities are purely storage services that exist separately from our HPC services. As such, you will now need to actively move the data from your RDF storage allocations to any compute allocations via Globus.

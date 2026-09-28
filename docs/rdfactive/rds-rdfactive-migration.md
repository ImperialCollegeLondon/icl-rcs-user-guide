# Migrating from the RDS to the RDF-Active

## Who is this page for

The RDF-Active replaces RDS Project spaces as the new location for large-scale research data storage at Imperial.

Use this page if your own or administer one of more RDS projects and need to move them to the RDF-Active or another storage service.

## Before you begin

* The RDS is still running while migrations continue.
    * Write access will be turned off at **Easter 2027** to facilitate the transfer of any remaining data.
    * The RDS will reach end of life in **July 2027**.
* You may choose to map one or several RDS projects into one allocation, depending on future access needs.
* RDF-Active uses **[ReCAP](../recap/index.md)** for administration.
* RDF-Active is [not directly accessible from HPC in the same way as the RDS](./rdfactive-faq.md#why-isnt-the-rdf-active-accessible-from-hpc-systems); HPC workflows may need data transfer steps, for example using [Globus](../globus/index.md).

## Key Terms for the RDF-Active

* **Research group**: administrative container used in ReCAP to manage access.
* **Storage allocation**: the actual RDF-Active storage space with quota and permissions.
* **Access group**: the permission group controlling who can use an allocation.
* **[ReCAP](../recap/index.md)**: the self-service portal used to manage research groups and allocations.

## Key differences between the RDS and the RDF-Active

| Topic | RDS | RDF-Active |
| ----- | --- | ---------- |
| Administration | [Self-service portal](https://selfservice.rcs.imperial.ac.uk/) | [ReCAP](../recap/index.md) |
| Organising unit | Project | Research group and storage allocation |
| Replication | Optional for additional charge | Dual-copy by default |
| Charging | Monthly based on usage | Pre-paid for 12 months based on requested quota |
| Credit Pools | RDS Credit Pools | [General purpose RCS Credits](../recap/rcs-credits.md) |
| HPC access | Direct via [CX3](../hpc/cluster-specification.md#cx3) | [Not directly accessible in the same way](./rdfactive-faq.md#why-isnt-the-rdf-active-accessible-from-hpc-systems); use a transfer workflow such as [Globus](../globus/index.md) |

## Reviewing your RDS Projects

We have started contacting [RDS Project](../rds/index.md) owners to provide them a list of their RDS projects for them to review. If you have not yet received a spreadsheet from us and would like a copy, please [raise a ticket with us](../support/index.md), and we will be happy to send it to you.

There are several options available for each RDS project:

* **Deletion:** If you no longer need the RDS Project space and have taken any necessary steps to preserve what data you require from it then you can request for the project to be deleted by [raising a ticket with us](../support/index.md). Please note that we can only take requests to close an RDS Project from the Project owner or designated administrator.
* **Migration to the RDF-Active:** Please see our [section below](#requesting-a-migration-from-the-rds-to-the-rdf-active) on requesting a migration to the RDF-Active.
* **Migration to Box:** We can help facilitate the move of your data to [Box](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/sharing-and-collaboration-tools/box-storage/). We recommend you contact the Box support team to first to explain what you wish to do. They will then take the necessary steps including setting up a research group area for you.

## Deciding on a migration layout

If you have more than one RDS project, you will need to consider what your migration layout will be:

* One RDS project to one storage allocation
* Many RDS projects to one storage allocation
* A mixed approach

### Examples

* **Example A**: one RDS project, one user group, same access needed after one migration -> one storage allocation
* **Example B**: three RDS projects with identical users and the research group leads wants them merged -> one storage allocation
* **Example C**: four RDS projects, two need restricted access and two are shared broadly -> multiple storage allocations (at least one per restricted access project)

## Requesting a migration from the RDS to the RDF-Active

Once you have decided on your migration layout, for each RDF-Active storage allocation you require, you will need to either:

* (**Preferred**) Follow the instructions for [Requesting a new storage allocation on the RDF-Active](./requesting-a-new-allocation.md) and provide a list of RDS Projects to migrate by attaching the marked up spreadsheet or a text file, or
* Use the [RDS to RDF-Active migration form](https://servicemgt.imperial.ac.uk/esc?id=sc_cat_item&sys_id=35ee36f83b3e66101feaf49a04e45a55&sysparm_category=52a4a8f21be62110557837b5464bcbd2). Please be aware that the form contains some outdated terminology. Specifically, any references to "**RDF Group Space**" should be interpreted as "**RDF-Active Storage Allocation**". Additionally, while the form suggests that only **G** and **P** codes may be used, this is not the case. Other charge codes, including departmental **F** codes, are also acceptable.

## What happens after you have requested a migration?

Once your request has come to us, we will contact you to discuss any specific requirements you have before setting up your storage allocation. We will then start migrating your data from the RDS to the RDF-Active; while your data is being migrated, you should continue to use the RDS as normal.

Once we have completed the initial transfer of your data from the RDS to your new RDF-Active storage allocation, we will contact you to arrange a suitable time to complete the migration. Please ensure that all users of the relevant RDS project(s) are informed and have disconnected from the RDS at the agreed time.
 
During the final migration stage, we will:

1. Remove user access to the RDS project(s).
2. Perform a final synchronisation of the project data from RDS to the RDF-Active storage allocation.
3. Confirm that the RDF-Active storage allocation is ready for use and that the migration has been successfully completed.

## What to do after migration?

* **Access Control**: Please make sure that everyone who needs access to your storage allocation has been granted the appropriate permissions. Access can be managed through [ReCAP](../recap/allocations.md), where you can add, remove, and review access for users as needed.
* **Update mapped drives/mount points**: You will need to update any devices that were using your RDS Project, to point to your new location on the RDF-Active. We have advice for [accessing the RDF-Active](./access/index.md) for each operating system.
* **Update any workflows using the HPC service**: The RDF-Active is not directly accessible from any of the HPC services; you will therefore need to update any workflows to move data around as necessary, using services such as [Globus](../globus/index.md).

## Need help?

If you need assistance with any of these steps, please [contact support](../support/index.md) and we'll be happy to help.
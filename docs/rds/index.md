# Research Data Store

The Research Data Store (RDS) is provided by the RCS for storing large volume of research data. You **don't** have to be registered in the HPC to use this service. Please visit the [Research Computing Service Getting Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) for information on how to register for the RDS.

!!! warning "Decommissioning Notice & Migration Requirement"
    The Research Data Store (RDS) is undergoing decommissioning as part of our transition to modern storage platforms. All active research data must be migrated to [RDF-Active](../rdfactive/index.md) or alternative Imperial storage services prior to service retirement.

## Decommissioning Timeline & What to Expect

### Phase 1: Transition & Active Migration
* **New Allocations:** New RDS project allocations and extension requests are restricted.
* **Active Migration:** Project leads are strongly advised to audit existing storage and begin transferring active project data to [RDF-Active](../rdfactive/rds-rdfactive-migration.md).

### Phase 2: Post-Deadline Restrictions
Past the decommissioning deadline, unmigrated project allocations will transition through the following states:
* **Read-Only Access Mode:** Unmigrated RDS project allocations will transition to **read-only access**. Users will remain able to view and download existing files, but no new data can be written or modified.
* **HPC Job Write Restrictions:** Compute jobs on HPC clusters will no longer have write permissions to RDS project directories. Ensure your batch scripts and workflows are updated to output to RDF-Active or temporary scratch space.
* **Account Archival & Final Retirement:** Allocations remaining unmigrated after the read-only grace period will be archived and scheduled for final removal in alignment with full service retirement in early 2027.

### Next Steps for Project Leads
1. **Audit Project Files:** Categorize data into active files (move to RDF-Active), historical data (move to RDF-Archive), and redundant/obsolete files (delete).
2. **Configure Access Groups:** Re-establish access groups using the [REsearch Computing Access Portal (ReCAP)](../recap/index.md).
3. **Migrate Data:** Follow the step-by-step instructions in the [RDS to RDF-Active Migration Guide](../rdfactive/rds-rdfactive-migration.md).

## Managing your Research Data

It is important to manage the data you store on the RDS to ensure that you comply with the University's and your funders Research Data Management policy, and also to reduce the frustration of locating data as your data volumes grow. The following links will introduce you to research data management, best practices and policies held by the College and many of the main grant funders.

* [Imperial Research Data Management Guidelines](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/)
* [Computational Methods Software & Data Guidance](https://www.imperial.ac.uk/research-and-innovation/support-for-staff/scholarly-communication/research-data-management/)

## Sensitive Data

The RDS (project allocations and HPC home areas) are not suitable for storing [personal or sensitive personal data](https://www.imperial.ac.uk/admin-services/secretariat/information-governance/data-protection/processing-personal-data/). If you are working with data of this type, please consult with the [Saving my files](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/saving-my-files/) page for advice on where your data can be stored.

## Using the Research Data Store

These pages contain the user guide for managing and accessing the RDS. It additionally provides advice on transferring data to/from the RDS. Please see the following pages for more information.

* [RDS Directory Paths](./paths.md)
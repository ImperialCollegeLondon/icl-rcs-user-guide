# Frequently Asked Questions for the RDF-Active

## Why do my access groups need to be set up again?

As part of our transition to RDF‑Active, we have adopted a new identity provider and self‑service portal ([REsearch Computing Access Portal](../recap/index.md)). This change ensures that RDF‑Active and all future RCS systems (such as HX2) will not be disrupted when the legacy identity services we currently rely on are decommissioned. However, this change has meant that access groups need to be setup again and we encourage you to take the opportunity to audit who can access what data.

## Why isn't the RDF-Active accessible from HPC systems?

Over five years of running the Research Data Store (**RDS**) taught us that, while centralising all research data on a single platform is convenient, supporting the full spectrum of workloads—especially **high‑performance computing (HPC)**—on one system is extremely challenging and often degrades service for everyone. By moving to dedicated, purpose‑built storage platforms (the **Research Data Facilities**), we can commission systems optimised for specific use cases (such as HPC). This shift also removes physical location constraints, giving us more flexibility in where these systems are deployed and scaled. 

**Globus** will be the primary method of moving data between platforms—the existing RDS, the RDF-Active, and new HPC platforms such as HX2—and we are adding additional endpoints to support this.

For research groups that require shared spaces on HPC platforms—for collaborative workflows, permissions‑managed access to common datasets, and consistent data governance—we will provide shared data storage areas upon request.
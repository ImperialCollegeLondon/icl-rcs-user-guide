# Frequently Asked Questions about the RDF-Active

## Why do my access groups need to be set up again?

As part of our transition to RDF‑Active and other new services at Imperial, we have adopted a new identity provider and self‑service portal called the *REsearch Computing Access Portal* or [ReCAP](../recap/index.md) for short. This change ensures that RDF‑Active and all future RCS systems, such as HX2, will not be disrupted when the legacy identity services we currently rely on are inevitably decommissioned. However, this change has meant that access groups need to be setup again. We encourage you to take this as an opportunity to audit who can access what data.

## How much do I need to know about ReCAP to use the RDF-Active storage?

Most users of the RDF-Active only need a cursory knowledge of ReCAP in order to actually use the facility. The only key piece of information needed is the storage allocation's file path, which can be obtained even by word of mouth instead of directly accessing ReCAP itself. 

Managers of research groups, however, should read and review our [documentation for ReCAP](../recap/index.md) as understanding and being fluent in [access control](../recap/research-groups.md#user-management) is a critical skill and part of their leadership responsibility to carry out.

## Why isn't the RDF-Active accessible from HPC systems?

As part of a wider restructuring of RCS services in order to improve system stability, targetted and intentional services, which should result in higher user satisfaction.

Over five years of operating the RDS service has shown us that, while hosting all research data on a single platform offers convenience, it is difficult to support the full range of research workloads effectively within one system. As usage has grown and diversified, this has increasingly led to competing demands that can affect the overall user experience.

In particular, we have found that the requirements of HPC workloads have become increasingly challenging to accommodate within the RDS environment. To address this, we are moving towards a set of dedicated, purpose-built storage platforms known as the Research Data Facilities (RDF). Each RDF platform is designed and optimised for specific use cases, allowing us to deliver better performance, functionality, and reliability for different research needs.

This approach also removes many of the physical infrastructure constraints associated with RDS, giving us greater flexibility in how and where services are deployed, expanded, and maintained.

To migrate your data out from the RDF-Active, [Globus](../globus/index.md) will be the primary method used whether the destination facility is the HX2, or any other of our newer and more robust facilities. (See [here](./rds-rdfactive-migration.md) for further details on migration.)

For research groups that require shared spaces on HPC platforms — for collaborative workflows, permissions‑managed access to common datasets, and consistent data governance — we will provide shared data storage areas upon request. To make a request, please [contact us](../support/index.md) via our ticket system.
///
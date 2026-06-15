# Frequently Asked Questions about the RDF-Active

/// details | Why do my access groups need to be set up again?
    type: faq
As part of our transition to RDF‑Active and other new services at Imperial, we have adopted a new identity provider and self‑service portal called the *REsearch Computing Access Portal* or [ReCAP](../recap/index.md) for short. This change ensures that RDF‑Active and all future RCS systems, such as HX2, will not be disrupted when the legacy identity services we currently rely on are inevitably decommissioned. However, this change has meant that access groups need to be setup again. We encourage you to take this as an opportunity to audit who can access what data.
///

/// details | How much do I need to know about ReCAP to use the RDF-Active storage?
    type: faq
Most users of the RDF-Active only need a cursory knowledge of ReCAP in order to actually use the facility. The only key piece of information needed is the storage allocation's file path, which can be obtained even by word of mouth instead of directly accessing ReCAP itself. 

Managers of research groups, however, should read and review our [documentation for ReCAP](../recap/index.md) as understanding and being fluent in [access control](../recap/research-groups.md#user-management) is a critical skill and part of their leadership responsibility to carry out.
///

/// details | Why isn't the RDF-Active accessible from HPC systems?
    type: faq
As part of a wider restructuring of RCS services in order to improve system stability, targetted and intentional services, which should result in higher user satisfaction.

Over five years of running the RDS service has taught us that while centralising all research data on a single platform is convenient, supporting the full spectrum of workloads on one system is extremely challenging and often degrades service for everyone. We noticed this was exasperated by the challenges around HPC workloads in particular. So we've decided to move to dedicated, purpose‑built storage platforms, called the Research Data Facilities (RDF), where we customise systems for specific use cases. This shift also removes physical location constraints suffered by the RDS, giving us more flexibility in where these systems are deployed and scaled.

To migrate your data out from the RDS, Globus will be the primary method used whether the destination facility is the RDF-Active, HX2, or any other of our newer and more robust facilities. (See [here](./rds-rdfactive-migration.md) for further details on migration.)

For research groups that require shared spaces on HPC platforms — for collaborative workflows, permissions‑managed access to common datasets, and consistent data governance — we will provide shared data storage areas upon request. To make a request, please [contact us](../support/index.md) via our ticket system.
///
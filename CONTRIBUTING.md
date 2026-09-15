# What is this file?

This file is not to be visible on the userdocs page i.e. not linked to or referenced throughout. This is strictly for RCS staff use for streamlining updating and adding in information at later dates. This file works as a signposting to speed up updating prior documentation when new features are released.

This is not guaranteed to be a comprehensive list of everywhere that needs updating, and is a continual work in progress.

## Contribution Workflow

1. Fork the main branch of the user guide repo.
2. In your forked repo, create a new feature branch from the main branch.
3. Make changes on that feature branch.
4. Create a PR in your forked repo from the feature branch into the main branch. Be sure it's the main branch of the user guide repo, rather than of your forked repo.

Be sure to re-fork the user guide repo every time you want to submit a new contribution, that way you're not working on an outdated version of the main branch.

## With every new facility/service release

### General

1. The general [landing page](index.md)'s list of available RCS services, to be extended.

### ReCAP

1. [ReCAP landing page](recap/index.md)'s list of supported services. Maybe remove "newly introduced" when describing the portal especially if the RDS portal is gone. Review text to be sure it reads correctly still.
2. [ReCAP allocations page](recap/allocations.md) needs a full review for every new ReCAP service added.
3. [ReCAP research groups](recap/research-groups.md) needs a quick review, with special attention paid to its [allocations management](recap/research-groups.md#managing-the-allocations-of-a-research-group) header.

### RDF-Active

1. The [RDF-Active's landing page](./rdfactive/index.md) should be more descriptive for what its storage service specialty is, e.g. versus the RDF-Secure. Perhaps offload/duplicate some of the explanation on this homepage onto a comparison page within the ReCAP section.
2. The [RDS to RDS-Active migration page](./rdfactive/rds-rdfactive-migration.md) should have an added section explaining how users can know if they want to migrate their stuff to the RDF-Active or an alternative service, e.g. the RDF-Secure. The newly added service should have a complimentary/identical section too.
3. For the [managing page](./rdfactive/managing/index.md), consider expanding it to explain how storage allocations can be migrated between various RCS storage systems.

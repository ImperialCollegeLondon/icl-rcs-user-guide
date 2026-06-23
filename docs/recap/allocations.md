# Managing Allocations

Please keep in mind, allocations exist under a research group. Therefore, management of allocations is an extension of [managing a research group](research-groups.md).

## Types of Allocations

There are various types of allocations, and can be understood under two main categories:
* The function of the allocation.
* The service the allocation is linked to.

### Functional-level Differences
From a functional standpoint, allocations can serve one of two purposes:
1. Storage allocations, which are linked to storage hardware.
2. Cluster allocations, which are linked to computing hardware and our services for [HPC workflows](../hpc/index.md).

Because the purpose of these allocations differs, so do the attributes visible to the users at the front end. However, both types have their GID visible and the users with access to that allocation visible.

### Service-level Differences
Currently, there is only one active service using allocations, the RDF-Active, so further details differentiating these allocations cannot be provided. 

However, there is documentation explaining how to manage your [RDF-Active storage allocations](../rdfactive/managing/index.md).

## Viewing your allocations

When you first login to [ReCAP](./access.md) you should see the home page with a *"Welcome to the Imperial Research Computing Access Portal"* message.

To see the allocations you are a member of:

* Click on **Groups** on the menu bar
* Click on **Allocations** on the drop-down list that appears

You should then be presented with a list of allocations for which you are a member of.

You can also see the allocations associated with a research group by [Viewing the Details of research group](./research-groups.md#viewing-details-of-a-research-group) and scrolling to the **Allocations** section.

## Viewing details of an allocation

### Viewing details from the list of allocations view

When looking at the list of allocations you're a member of, you can click on the **ID** number of the allocation you're interested in to view its details.

### Viewing details from the Research Group details view

In the research group details view, find the row for the allocation you're interested in and click its corresponding **Action** button to view the allocation's details.

## User Management

!!! Note
    It is important to note that adding or removing a user from an allocation changes their access to the associated resource. For example, adding a user to an RDF‑Active storage allocation grants them membership in the corresponding filesystem access group, allowing them to access the files in that allocation. Because these changes need applying outside the ReCAP service, it may take some time — potentially several hours — for the updated access permissions to propagate to the resource.

### Adding a user to an allocation

A user **MUST** be added to your [research group](./research-groups.md) before being adding to an allocation. If the user is not yet a member of the research group, please follow [the steps required](./research-groups.md#adding-users-to-your-research-group). Adding a user to a research group will also give you the option to add them to the allocation at the same time.

If a user IS already a member of the research group but is not yet a member of the allocation:

* Follow the steps needed to [view the details of an allocation](#viewing-details-of-an-allocation).
* Scroll down to **Users** in the Allocation.
* Click the **Add Users** button at the top of that section.
* Use the tick box to select all the users you would like to add to the allocation.
* Click the **Add Selected Users to Allocation** button.

*Please note: as already mentioned, it can take several hours for changes to be propagated on the corresponding resource.*

### Removing a user from an allocation

If you only want to remove a user from a single allocation:

* Follow the steps needed to [view the details of an allocation](#viewing-details-of-an-allocation).
* Scroll down to **Users** in Allocation.
* Click the **Remove Users** button at the top of that section.
* Use the tick box to select all the users you would like to remove from the allocation.
* Click the **Remove Selected Users to Allocation** button.

If you would like to remove a user from all the allocations associated with a research group, then consider [removing the user from the research group itself](./research-groups.md#removing-users-from-your-research-group) as this removes users automatically from any associated allocation.

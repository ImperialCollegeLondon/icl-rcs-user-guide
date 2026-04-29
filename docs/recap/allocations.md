# Managing Allocations

## Viewing your allocations

When you first login to [RECAP](./access.md) you should see the home page with the *Welcome to the Imperial Research Computing Access Portal* message. 

To see the allocations you are a member of:

* Click on **Groups** on the menu bar
* Click on **Allocations** on the drop-down list that appears

You should then be presented with a list of allocations for which you are a member of. 

You can also see the allocations associated with a Research Group by [Viewing the Details of Research Group](./research-groups.md#viewing-details-of-a-research-group) and scrolling to the **Allocations** section.

## Viewing details of an allocation

### From the list of allocations view

In the list of allocations for which you are a member of, you can click the **ID** number corresponding to the allocation detail you wish to view.

### From the Research Group details view

You can the view the details of any allocation by clicking the **Action** button in the row corresponding to the allocation in question.

## User Management

!!! Note
    It is important to note that adding or removing a user from an allocation changes their access to the associated resource. For example, adding a user to an RDF‑Active storage allocation grants them membership in the corresponding filesystem access group, allowing them to access the files in that allocation. Because these changes are applied outside the RECAP portal, it may take some time—potentially several hours—for the updated access permissions to propagate to the resource.

### Adding a user to an allocation

A user **MUST** be added to your [Research Group](./research-groups.md) before being adding to an allocation. Adding a user to a Research Group will also give you the option to add them to the allocation at the same time, so following the steps under [Adding users to your Research Group](./research-groups.md#adding-users-to-your-research-group) if the user is not yet a member of the Research Group.

If a user IS already a member of the Research Group but is not yet a member of the allocation:

* Follow the instructions under [Viewing details of an allocation](#viewing-details-of-an-allocation) section to view the details of an allocation.
* Scroll down to Users in Allocation.
* Click the Add Users button at the top of that section.
* Use the tick box to select all the users you would like to add to the allocation.
* Click the **Add Selected Users to Allocation** button.
* *Note as already mentioned, it can take several hours for changes to be propagated on the corresponding resource.*

### Removing a user from an allocation

If would like to remove a user from all allocations and the research group, please following the instructions on the [Removing users from your Research Group](./research-groups.md#removing-users-from-your-research-group) page as this will automatically remove the user from any associated allocation.

If you only want to remove a user from a single allocation:

* Follow the instructions under [Viewing details of an allocation](#viewing-details-of-an-allocation) section to view the details of an allocation.
* Scroll down to Users in Allocation.
* Click the Remove Users button at the top of that section.
* Use the tick box to select all the users you would like to remove from the allocation.
* Click the **Add Selected Users to Allocation** button.